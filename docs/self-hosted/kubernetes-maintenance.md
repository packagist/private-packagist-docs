# Maintenance
## Private Packagist Self-Hosted (Kubernetes)

### Updates

By default Replicated checks for updates every 4 hours and you can install them from the dashboard once they are available.
You can configure this behaviour under “Configure automatic updates” on the dashboard.

The [Private Packagist Self-Hosted Changelog](https://packagist.com/docs/self-hosted/changelog) details all new features,
behavior changes and major bugfixes each release introduces.

New versions that ask you to upgrade KOTS (Kubernetes Off-The-Shelf), the Replicated tool used to distribute Private Packagist,
require that you download and rerun the install script via the command below.

#### Update KOTS for Private Packagist Self-Hosted Kubernetes without an existing cluster

Please note that running the command will take a while and that Private Packagist and the Replicated Management Console will become temporarily unavailable.

```
curl -sSL https://k8s.kurl.sh/privatepackagistkots | bash -s
```

#### Update KOTS for Private Packagist Self-Hosted Kubernetes in an existing cluster

```
curl https://kots.io/install | bash
```
