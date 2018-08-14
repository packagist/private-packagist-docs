# Troubleshooting
## Private Packagist Enterprise

#### devicemapper I/O errors on docker service
If you are using RedHat Enterprise Linux RHEL 7.x, try setting the option
`MountFlags=slave` in the Service section of the docker service configuration
file (usually `usr/lib/systemd/system/docker.service`). After making this
change restart the docker service and make sure to reload the system
configuration.
