## Troubleshooting

This section contains solutions to common problems that may arise during the setup and usage of the plugin.

#### Debug Logging
Use the docker plugin ls command.
$ docker plugin ls
ID               NAME               DESCRIPTION                                    ENABLED
8a4231e33dc8     simplivity:1.0     Docker Volume plugin for HPE SimpliVity         true

Note the ID column in the output. Use this to glob the directory to the log of the plugin.
less /var/lib/docker/plugins/8a4231e33dc8*/rootfs/var/log/hpe-flexvolume-plugin.log

Sometimes it is useful to get more verbose output from the plugin. To enable logging for REST calls made to SimpliVity Container Provider from Volume Plugin, add below flag
```
LOG_LEVEL=trace in 'docker plugin set <plugin_alias> SCOPE=<local/global> PLUGIN_TYPE=simplivity PROVIDER_IP=<Simplivity_CP_IP> PROVIDER_USERNAME=admin PROVIDER_PASSWORD=admin LOG_LEVEL=trace INSECURE=true PROVIDER_PORT=<Simplivity_CP_PORT>'
```

#### Logs for the plugin

Logs of plugin provides useful information on troubleshooting issue/error further. On Ubuntu, grep for the `plugin id` in the logs , where the `plugin id` can be identified by:

`$ docker-runc list`

Plugin logs will be available in system logs (e.g. `/var/log/syslog` on Ubuntu).

On RHEL and CentOS, issue `journalctl -f -u docker.service` to get the plugin logs.

#### Containerized Plugin

```
$docker logs -f <container id of Plugin>
Getting container id of plugin: docker ps -a | grep hpe
```
