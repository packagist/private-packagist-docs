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
interface to generate the support bundle on your host system: `replicatedctl support-bundle`

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

#### Invalid x509 keypair: tls: failed to find any PEM data in key input

If you get this error message when upload your SSL certificate you uploaded an
incorrect certificate file. Please note that if you need to include
intermediate certificates, the order of certificates in the certificate file
needs to be from leaf to root. If the order of certificates does not match this
format Replicated will fail to recognize your certificate.

#### Composer install/update fails with SSL error: failed to enable crypto

If your composer commands fail with a TransportException similar to the one
below after you successfully uploaded your certificate then this indicates that
your local system does not trust or recognize the certificate. Accessing your
installation in a browser may still work because browsers often use their own
sets of CAs/certificates which are often more complete and up to date than the
certificate collection on your operationg system which is used by PHP and
Composer.

```
[Composer\Downloader\TransportException]
The "https://repo.acme-website.com/acme/packages.json" file could not be downloaded: SSL operation failed with code 1. OpenSSL Error messages:
error:14090086:SSL routines:ssl3_get_server_certificate:certificate verify failed
Failed to enable crypto
failed to open stream: operation failed
```

You either need to add your certificate or the corresponding CA'sroot
certificate to your operating system's certificate store, or more commonly
there is an intermediate certificate required for your system to trust the key
you uploaded. The intermediate key should be supplied by the provider of the
SSL certificate and may also already be present in your browser explaining why
you can open Private Packagist pages in your browser without an error. Please
see the section about `Invalid x509 keypair` for more information on how to add
your intermediate certificates to your certificate file.

#### Ready state command canceled: context deadline exceeded

If you move the application behind a HTTP proxy or if you change docker's config
or your host system's network configuration in a way that the internal docker IPs
change then replicated won't be able to check the availability of the other containers anymore.
This can be fixed by rerunning the original replicated install script. This will
leave your current installation in tact but rewrite the main config files and therefore
update the host IP and the NO_PROXY environment.
