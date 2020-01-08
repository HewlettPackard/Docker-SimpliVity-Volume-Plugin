# HPE SimpliVity Volume Plugin for Docker

HPE Docker Volume Plugin is an open source project that provides persistent storage and features for your containerized applications using HPE SimpliVity.

Currently, this Volume Plugin for Docker supports popular container platform ie. Docker. We will be supporting Kubernetes distributions through the [Flexvolume Driver](https://github.com/hpe-storage/flexvolume-driver) and [Kubernetes Dynamic Provisioner](https://github.com/hpe-storage/k8s-dynamic-provisioner) in upcoming releases.

## Install and Quick Start instructions

* Review the [System Requirements](/docs/system-reqs.md) before installing the plugin
* Check out the [Quick Start Guide](/docs/quick_start_guide.md) for deploying the **HPE Docker Volume Plugin** in [Docker](/docs/quick_start_guide.md#docker)

## Supported Features

* volume features
  * restore
  * clones
  * consistency group(grouping of volumes)
  * mount_conflict_delay
  * concurrent volume access
  * multiple backups via backup policy
  * file system permissions and ownership

## Troubleshooting

Troubleshooting issues with the plugin can be performed using these [tips](/docs/troubleshooting.md)


## Limitations
- List of issues around the containerized version of the plugin is present in https://github.com/HewlettPackard/Docker-SimpliVity-Volume-Plugin/issues

- Shared volume support is present for containers running on the same host.

- For upgrading the plugin from older version to the current released version, user needs to unmount all the volumes and follow the standard
 upgrade procedure described in docker guide.
 
- User can list the backups associated with the volume by inspecting the volume 

```Inspect the volume to verify if the backups got created
docker volume inspect <volume_name>. This should display backups associated to the volume which can be selected for the Volume Restore in the restoreOf option.

```
