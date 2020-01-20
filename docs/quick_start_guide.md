# Deployment methods for HPE SimpliVity Volume Plugin-in

## HPE SimpliVity Docker volume Plugin can be deployed in following methods:

* [Quick Start Guide for Docker Engine Managed Plugin](#docker)

## Quick Start Guide for Docker Engine Managed Plugin <a name="docker"></a>

Steps for Deploying the Managed Plugin (HPE SimpliVity Volume Plug-in for Docker) in a Docker Engine environment.

### **Prerequisite packages to be installed on host OS:**

#### RHEL/CentOS 7.3 or later:

1. Install the iSCSI and Multipath packages

```
$ yum install -y iscsi-initiator-utils device-mapper-multipath
```
2. Install VMware tools on all worker nodes.
```
$ yum install -y open-vm-tools
```
3. Add "disk.EnableUUID = "TRUE"" parameter for all worker nodes VM’s

    a.	Locate the virtual machine for which you are enabling the disk UUID attribute, and power off the virtual machine.

    b.	After power-off, right-click the virtual machine, and choose Edit Settings.

    c.	Click VM Options tab, and select Advanced.

    d.	Click Edit Configuration in Configuration Parameters.

    e.	Click Add parameter.

    f.	In the Key column, type disk.EnableUUID.

    g.	In the Value column, type TRUE.

    h.	Click OK and click Save.

    i.	Power on the virtual machine

4. Restart the Docker daemon

```
$ systemctl daemon-reload
$ systemctl restart docker.service
```

#### Ubuntu 18.04 LTS or later:

1. Install the iSCSI and Multipath packages

```
$ apt-get install -y open-iscsi multipath-tools xfsprogs
```

2. Install VMware tools on all worker nodes.
```
$ sudo apt-get install open-vm-tools-lts-trusty
```

3. Repeat steps 3 and 4 from RHEL/CentOS section.

Now the systems are ready to setup the HPE SimpliVity Volume Plug-in for Docker.

### Next Steps:

### Running SimpliVity Container Provider as a container:
1. First create 3 directories at below paths in the VM where you are planning to run this container.
```
$ mkdir -p /simplivity/var/private/config/current/
$ mkdir -p /simplivity/var/private/log/group/
$ mkdir -p /simplivity/var/private/temp/
```
2. Now create appint-container-provider.default file to configure the SimpliVity Container Provider properties.
```
$ vi /simplivity/var/private/config/current/appint-container-provider.default
```   
3. Configure Datacentre, Datastore, Esxi Hostname, Omnistack IP and Vcenter server IP. Below is a sample config file.

   docker.volume.datacenter=Barcelona
   
   docker.volume.datastore=SVT_Barcelona01
   
   docker.volume.hostname=omnicube304240.cloud.local
   
   docker.volume.omnistack.hostname=10.30.4.245
   
   docker.volume.vcenter.server=10.20.0.58

4. Run the following Docker commands to create the HPE SimpliVity Container Provider container.
```
$ sudo docker pull hpesimplivity/simplivity-container-provider:1.0
$ sudo docker run -v /simplivity/var/private/config/current/:/simplivity/var/private/config/current/ \
-v /simplivity/var/private/log/group/:/simplivity/var/private/log/group/ \
-v /simplivity/var/private/temp/:/simplivity/var/private/temp \
-p 9080:9080 -e JAVA_OPTIONS="-Djetty.port=9080" \
-e "TZ=America/Los_Angeles" \
hpesimplivity/simplivity-container-provider:1.0
```

**IMPORTANT**

Before enabling the plugin, validate the following:

* simplivity-container-provider container is in running state.
* Proper zoning between the docker host(s) and the SimpliVity Container Provider.

---

3. Run the following commands to install the plugin:


**RHEL/CentOS**

```
$ docker plugin install --disable --grant-all-permissions --alias simplivity hpesimplivity/simplivity:<version>
$ docker plugin set simplivity SCOPE=local PLUGIN_TYPE=simplivity PROVIDER_IP=<container_provider_IP> PROVIDER_USERNAME=admin PROVIDER_PASSWORD=admin INSECURE=true PROVIDER_PORT=9080
$ docker plugin enable simplivity
```

4. Confirm the plugin is successfully installed by

```
$ docker plugin ls
```

## Usage <a name="usage"></a>

For usage go to:

[Usage](/docs/usage.md)
