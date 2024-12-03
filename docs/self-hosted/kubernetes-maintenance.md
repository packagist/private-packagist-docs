# Maintenance
## Private Packagist Self-Hosted (Kubernetes)

### Updates

By default Replicated checks for updates every 4 hours and you can install updates from the dashboard in the Replicated
Management Console on port 8800 once they are available.
You can configure this behavior under “Configure automatic updates” on the dashboard.

The [Private Packagist Self-Hosted Changelog](https://packagist.com/docs/self-hosted/changelog) details all new features,
behavior changes and major bugfixes each release introduces.

New versions that ask you to upgrade KOTS (Kubernetes Off-The-Shelf), the Replicated tool used to distribute Private Packagist,
require that you download and rerun the install script via the command below.

#### Update KOTS for Private Packagist Self-Hosted Kubernetes in a Kubernetes cluster installed with kURL

Please note that running the command will take a while as it will also update Kubernetes and other dependencies of Private Packagist Self-Hosted.
Private Packagist and the Replicated Management Console will become temporarily unavailable.

```
curl -sSL https://k8s.kurl.sh/privatepackagistkots | bash -s
```

#### Update KOTS for Private Packagist Self-Hosted Kubernetes in an existing cluster

```
curl https://kots.io/install | bash
kubectl kots admin-console upgrade -n NAMESPACE
```

Replace `NAMESPACE` with the namespace in your cluster where KOTS is installed.

### Update or replace the SSL certificate

The following applies only to kURL installations. To update or replace the certificate, you must first restore the ability 
to upload new TLS certificates and then reupload the certificate through the user interface. Please refer to 
the [Replicated documentation](https://docs.replicated.com/enterprise/updating-tls-cert#update-custom-tls-certificates) 
for detailed instructions.

