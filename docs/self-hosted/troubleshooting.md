# Troubleshooting
## Private Packagist Self-Hosted

### System logs and information

If you contact us to request support please send along a support bundle file.
This file can be generated with Replicated and contains all relevant system
and container logs which we may require to analyze problems with your Private
Packagist Self-Hosted installation. You are welcome to download and inspect
the logs in this file yourself as well.

#### Generating a support bundle

You can generate a support bundle from the Replicated Management Console on
port 8800 by navigating to the Support tab and clicking on the "Download
Support Bundle" button. Alternatively you can use replicated's command line
interface to generate the support bundle on your host system:

`replicatedctl support-bundle`

In case generating the support bundle via replicated's command line fails then you can run the following command:
```
docker run -it --rm \
--name support-bundle \
--volume $PWD:/out \
--volume /var/run/docker.sock:/var/run/docker.sock \
--net host --pid host --workdir /out  \
-e HTTP_PROXY -e HTTPS_PROXY -e NO_PROXY \
replicated/support-bundle \
generate \
--channel-id 6e3299f45997e91132719014584b06e4
```
This command will generate a support bundle with less information than the command above, but it may still be helpful if the replicated command does not work at all. It will ask you interactively to upload the file, but please contact us regardless of whether you upload the bundle through or want to send it to us separately, as we are not notified automatically.

#### Manually inspecting logs
If you are unable to generate a support bundle through either of these mechanisms
please include the output of the command `docker ps -as` which will list all
containers including containers which terminated because of errors. Further
you can then retrieve logs for each of the containers using `docker logs <container-id`.
Of particular interest are the logs of the containers replicated,
replicated-operator, repo, worker and ui.

### Common Issues

#### Waiting for app to report ready ...

Once you start Private Packagist Self-Hosted from the Replicated Management
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

#### Reset Replicated Management Console authentication

If you cannot log into the Replicated Management Console anymore, then you can
reset LDAP and password authentication on the host system by running the
following command:

```
replicatedctl console-auth reset
```

#### Issues with Multi-factor Authentication

If you are having problems setting up MFA, or are unable to login via MFA, with
your generated codes, there may be a time-drift issue with either the
Self-hosted Private Packagist server or the device you are using to generate the
codes.

To make sure that the Self-hosted Private Packagist server is synchronized to
the correct time, you should check that both the current server time and
timezone are set to correct values. If you can enable Network Time Protocol
(NTP) for the server, we also recommend doing that.

The methods for doing so will vary depending on the underlying server Operating
System.

> Please be aware that offline-based TOTP hardware can drift up to a few minutes
> a year. As Private Packagist only allows time-drift of up to one (1) minute, we
> recommend using TOTP devices that have the ability to stay synchronized with
> the correct time (such as a phone, or re-programmable TOTP hardware devices).
