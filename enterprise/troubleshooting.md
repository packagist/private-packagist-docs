# Troubleshooting
## Private Packagist Enterprise

### System logs and information

If you contact us to request support please send along a support bundle file.
This file can be generated with Replicated and contains all relevant system
and container logs which we may require to analyze problems with your Private
Packagist Enterprise installation. You are welcome to download and inspect
the logs in this file yourself as well.

#### Generating a support bundle

You can generate a support bundle from the Replicated Management Console on
port 8800 by navigating to the Support tab and clicking on the "Download
Support Bundle" button. Alternatively you can use replicated's command line
interface to generate the support bundle on your host system: `replicatectl support-bundle`

#### Manually inspecting logs
If you are unable to generate a support bundle through either of these mechanisms
please include the output of the command `docker ps -as` which will list all
containers including containers which terminated because of errors. Further
you can then retrieve logs for each of the containers using `docker logs <container-id`.
Of particular interest are the logs of the containers replicated,
replicated-operator, repo, worker and ui.

### Common Issues

#### Waiting for app to report ready ...

Once you start Private Packagist Enterprise from the Replicated Management
Console the dashboard will display a spinning icon and let you know that it is
waiting for the application. Replicated is now waiting for the web health check
of Private Packagist to return a 200 OK response. If this message does not
disappear in a timely manner, the health check is most likely failing. You can
view details on the health check report by inspecting the logs for the `ui`
container (not the `replicated-ui` container which is only responsible for the
management console). Look up the container id using `docker ps -as` and then
view the logs using `docker logs <container-id>`. If the health check is
failing the log will contain a JSON structure with all system checks and their
respective status and error messages.
 
#### devicemapper I/O errors on docker service

If you are using RedHat Enterprise Linux RHEL 7.x, try setting the option
`MountFlags=slave` in the Service section of the docker service configuration
file (usually `usr/lib/systemd/system/docker.service`). After making this
change restart the docker service and make sure to reload the system
configuration.
