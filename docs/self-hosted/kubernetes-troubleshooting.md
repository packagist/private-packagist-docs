# Troubleshooting
## Private Packagist Self-Hosted (Kubernetes)

#### Private Packagist Self-Hosted update requires KOTS upgrade
If the dashboard shows: 

'This version of Private Packagist Self-Hosted requires a version of KOTS that is different than what you currently have installed.'

Please refer to the "[Updates](./kubernetes-maintenance.md#updates)" section in our maintenance guide.

#### Generating a support bundle

You can generate a support bundle from the Replicated Management Console on port
8800 by navigating to the Troubleshoot tab, clicking "Generate a support bundle",
and selecting "Analyze".
Once the analysis is done, either download the bundle and manually send it to us
or click on the send icon which will send us the bundle. Please always notify us
if you send us a support bundle!

#### Issues with Multi-factor Authentication

If you are having problems setting up MFA, or are unable to log in via MFA, with
your generated codes, there may be a time-drift issue with either the
Self-hosted Private Packagist server or the device you are using to generate the
codes.

To make sure that the Self-hosted Private Packagist server is synchronized to
the correct time, you should check that both the current server time and
timezone are set to the correct values. If you can enable Network Time Protocol
(NTP) for the server, we also recommend doing that.

The methods for doing so will vary depending on the underlying server Operating
System.

> Please be aware that offline-based TOTP hardware can drift up to a few minutes
> a year. As Private Packagist only allows a time-drift of up to one (1) minute, we
> recommend using TOTP devices that have the ability to stay synchronized with
> the correct time (such as a phone, or re-programmable TOTP hardware devices).

#### Issues with Reverse-Proxy running in front of the Kubernetes Cluster

Please follow the instructions below, if you are experiencing problems with the reverse-proxy not being able to connect to
the cluster and encountering errors like this:
```
Peer closed connection in SSL handshake (104: Connection reset by peer) while SSL handshaking to upstream
```

Ensure that the SNI (Server Name Indication) TLS Extension is properly passed in requests to the cluster
for SNI to work correctly on the ingress. This is not the case when using IPs in `proxy_pass` on the reverse proxy and will result in an SSL handshake error.

To pass the SNI hostname from the incoming request to the upstream server, apply the following settings when using
NGINX as a reverse-proxy:
``` 
proxy_ssl_name $host;
proxy_ssl_server_name on;
```

If you are using different hostnames on the upstream and on the reverse-proxy, set the value in the
`proxy_ssl_name` directive to the corresponding hostname of the upstream server.

Please consult the documentation of other reverse-proxy servers to achieve the same result.
