## Usage of the HPE SimpliVity Volume Plug-in for Docker

The following guide covers many of the options used for provisioning volumes and volume management within standalone Docker environments.

## Within Docker<a name="docker_usage"></a>
The following section covers the supported actions for the **HPE SimpliVity Volume Plug-in** within a **Docker** environment.

  * [Creating a basic HPE SimpliVity volume](#basic)
  * [Volume optional parameters](#options)
  * [Deleting a volume](#delete)
  * [Listing volumes](#list)
  * [Inspecting a volume](#inspect)
  * [Mounting a volume](#mount)
  * [Unmounting a volume](#unmount)
  * [Cloning a volume](#clone)
  * [Enabling file permissions and ownership](#file-permission-owner)
  * [Creating volumes with backup policy](#create-volume-via-backup-policys)
  * [Restoring volumes](#restore-vol)
  * [Display help on usage](#usage-help)
  * [Display available backends and their status](#available-backups)

### Creating a basic HPE SimpliVity volume<a name="basic"></a>
```
$ sudo docker volume create -d simplivity --name <vol_name>
```

### HPE SimpliVity Docker Volume parameters<a name="options"></a>
The **HPE SimpliVity Docker Volume Plug-in** supports several optional parameters that can be used during volume creation:

- **size** -- specifies the desired size in GB of the volume. If size is not specified during volume creation, it defaults to 100 GB.

- **mountConflictDelay** -- specifies period in seconds to wait for a mounted volume to gracefully unmount from a node before it can be mounted to another. If graceful unmount doesn't happen within the specified time then a forced cleanup of the VLUN is performed so that volume can be remounted to another node.

- **cgName** -- name of Consistency Group(VM) which holds the one or more volumes(optional)

- **description** -- text to be added to volume description (optional)

- **cloneOf** -- name of Docker Volume to create a clone of

- **cgFlag** -- if sets to true, indicates to delete all the other metadata files in case of cloning the simpliVity volume

- **restoreOf** -- name of the SimpliVity backup volume to restore

- **fsOwner** -- user ID and group ID that should own root directory of file system.

- **fsMode** -- mode of the root directory of file system to be specified as octal number. 

- **protectionTemplate** -- name of the protection template (default is Fixed Default Backup Policy)

- **help** -- displays usage help and backend initialization status.

The following is an example Docker command creating 50 GB volume:
```
$ docker volume create -d simplivity --name <vol_name> -o size=50
```

### Enabling file permissions and ownership<a name="file-permission-owner"></a>
1. To set permissions of root directory of a file system:
```
$ docker volume create -d simplivity --name <volume-name> -o fsMode=<octal-number-specified-in-chmod>
```

2. To set ownership of root directory of a file system:
```
$ docker volume create -d simplivity --name <volume-name> -o fsOwner=<UserId>:<GroupId>
```

### Creating volume with backup policy to get scheduled backups><a name="create-volume-via-backup-policy"></a>
```
$ docker volume create -d simplivity --name <volume-name> -o protectionTemplate=<backup_policy_name>
```

### Deleting a volume<a name="delete"></a>
```
$ docker volume rm <vol_name>
```

### Listing volumes<a name="list"></a>
```
$ docker volume ls
```

### Inspecting a volume<a name="inspect"></a>
```
$ docker volume inspect <vol_name>
```

### Mounting a volume<a name="mount"></a>
Use the following command to mount a volume and start a bash prompt:
```
$ docker run -it -v <vol_name>:/<mount_point>/ --volume-driver simplivity <image_name> bash
```

e.g. $ docker run -it -v devVolume:/data --volume-driver simplivity alpine /bin/sh

The image used for mounting can be any image located on https://hub.docker.com/ or
the local filesystem. See https://docs.docker.com/v1.8/userguide/dockerimages/
for more details.

### Unmounting a volume<a name="unmount"></a>
Exiting the bash prompt will cause the volume to unmount:
```
/ exit
```

The volume is still associated with a container at this point.

Run the following command to get the container ID associated with the volume:
```
$ sudo docker ps -a
```

Then stop the container:
```
$ sudo docker stop <container_id>
```

Next, delete the container:
```
$ sudo docker rm <container_id>
```

Finally, remove the volume:
```
$ sudo docker volume rm <vol_name>
```

### Creating a clone of a volume <a name="clone"></a>
```
$ docker volume create -d simplivity --name <target_vol_name> -o cloneOf=<source_vol_name> -o cgFlag=<True/False>
```

### Restoring volumes <a name="restore-vol"></a>
```
$ docker volume create -d simplivity --name <target_vol_name> -o restoreOf=<backup_vol_name> -o cgName=<source_vol_name> -o cgFlag=<True/False>
```

### Displaying help on usage <a name="usage-help"></a>
```
$ docker volume create -d simplivity -o help
```

### Displaying available backups <a name="available-backups"></a>
```
$ docker volume inspect <source_vol_name>
```
