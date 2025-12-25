# Troubleshooting
## Private Packagist Self-Hosted (Kubernetes)

#### Private Packagist Self-Hosted update requires KOTS upgrade
If the dashboard shows: 

'This version of Private Packagist Self-Hosted requires a version of KOTS that is different than what you currently have installed.'

Please refer to the "[Updates](./kubernetes-maintenance.md#updates)" section in our maintenance guide.

#### Installer fails during update

If the kURL installer exits with the error message `Kurl has unhealthy Pod(s)` before starting the actual upgrade, it usually means
there are older pods stuck in an invalid state from previous runs. You can remove these with this command and try again:

```
kubectl delete pods --field-selector status.phase=Failed -n kurl
```

#### Generating a support bundle

You can generate a support bundle from the Replicated management console on port
8800 by navigating to the _Troubleshoot_ tab, clicking _Generate a support bundle_,
and selecting _Analyze_.
Once the analysis is done, either download the bundle and manually send it to us
or click on the send icon which will send us the bundle. Please always notify us
if you send us a support bundle!

In cases where the Replicated management console isn't accessible you can also generate a [host support bundle](https://docs.replicated.com/vendor/support-host-support-bundles)
using the following command and send it to us:
```
kubectl support-bundle https://raw.githubusercontent.com/replicatedhq/troubleshoot-specs/main/host/default.yaml
```

Alternatively, you can also download the YAML file first and run `kubectl support-bundle PATH/FILE.yaml`.

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

The following examples assume you are using nginx as a reverse-proxy. Please consult the documentation of other 
reverse-proxy servers to achieve the same result.

Ensure that the SNI (Server Name Indication) TLS Extension is properly set for requests to the ingress controller of the Kubernetes Cluster.
This is not the case when using IPs in `proxy_pass` and will result in an SSL handshake error.

To pass the SNI hostname from the incoming request to the upstream server, add the following directives to nginx:
``` 
proxy_ssl_name $host;
proxy_ssl_server_name on;
```

If you are using different hostnames on the upstream and on the reverse-proxy, set the value in the
`proxy_ssl_name` directive to the corresponding hostname of the upstream server.

#### Issues after changing the Private Packagist Self-Hosted domain name

If you've changed the domain name used to access your Private Packagist Self-Hosted Kubernetes installation, you'll need to clear the Composer endpoint Redis cache.

**Important:** Please note that there are multiple databases created in your Redis instance, and that Redis is not solely used for caching.
Be careful not to flush any of the other databases.

If you are not using the built-in Redis database, check your internal documentation how to connect to the Redis instance.

```
kubectl exec -it redis-0 -- redis-cli -p 9869
select 5
flushdb
```

Afterwards, run `composer update mirrors` to make sure all repository references are up-to-date. 

#### Issues with internal hostname resolution

Kubernetes clusters use their own DNS resolution mechanism. Even if the host server can resolve local hostnames (through 
local DNS servers or `/etc/resolv.conf`), these names are not resolvable within the cluster.

You can configure additional hostnames to be resolved by the cluster by following the instructions below.

Make a backup of the current CoreDNS config to a yaml file in case you need to revert changes or want to keep it as
a reference:
```
kubectl -n kube-system get configmap coredns -o yaml > coredns-config.yaml
```

Start editing the CoreDNS config by issuing the following command:
```
kubectl -n kube-system edit configmap coredns
```

This will open the current CoreDNS config in your default editor. Add all additional hostnames with corresponding IPs to the `hosts` config block. 
If the `hosts` config block doesn't exist yet, please add it. 

**Important:** Add the `fallthrough` entry as the last entry in order to resolve all other hostnames that are not listed in the `hosts` config block!

The full configuration should look similar to this:
```
data:
  Corefile: |
    .:53 {
        errors
        health {
           lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
           pods insecure
           fallthrough in-addr.arpa ip6.arpa
           ttl 30
        }
        prometheus :9153
        forward . /etc/resolv.conf {
           max_concurrent 1000
        }
        hosts {
          10.1.2.3 your-gitlab-server-hostname.local
          fallthrough
        }
        cache 30
        loop
        reload
        loadbalance
    }
```


Restart CoreDNS to apply the changes:
```
kubectl -n kube-system rollout restart deployment coredns
```

To verify that the configured hostnames can now be correctly resolved, use this command:
```
kubectl exec -it $(kubectl get pods -o name | grep worker | head -1 | cut -d'/' -f2) -- nslookup your-gitlab-server-hostname.local
```
