# Maintenance
## Private Packagist Self-Hosted (Kubernetes)

## kURL installations

### Updates

This section describes how to update a Private Packagist Self-Hosted in a Kubernetes cluster installed with [kURL](kubernetes-embedded).
This section is not relevant if you installed the application with [Helm](kubernetes-helm).

By default Replicated checks for updates every 4 hours and you can install updates from the dashboard in the Replicated
Management Console on port 8800 once they are available.
You can configure this behavior under “Configure automatic updates” on the dashboard.

The [Private Packagist Self-Hosted Changelog](https://packagist.com/docs/self-hosted/changelog) details all new features,
behavior changes and major bugfixes each release introduces.

You can safely bump multiple versions at once, it's not required to install individual versions consecutively. If we ever 
release a version that needs to be installed specifically, we will make sure to communicate this first.

We recommend backing up your database before each update. We cannot guarantee that downgrading to a previous release will always work
due to database migrations. In case of upgrade failure, the most reliable way to rollback to the previous version is via a snapshot.

New versions that ask you to upgrade KOTS (Kubernetes Off-The-Shelf), the Replicated tool used to distribute Private Packagist,
require that you download and rerun the install script via the command below.

#### Update KOTS for Private Packagist Self-Hosted Kubernetes in a Kubernetes cluster installed with kURL

Please note that running the command will take a while as it will also update Kubernetes and other dependencies of Private Packagist Self-Hosted.
Private Packagist and the Replicated Management Console will become temporarily unavailable.

```
curl -sSL https://k8s.kurl.sh/privatepackagistkots | bash -s
```

### Update or replace the SSL certificate

To update or replace the certificate, you must first restore the ability to upload new TLS certificates and then reupload
the certificate through the user interface. Please refer to the
[Replicated documentation](https://docs.replicated.com/enterprise/updating-tls-cert#update-custom-tls-certificates)
for detailed instructions.

## Helm installations

### Updates

If you installed Private Packagist Self-Hosted with Helm into your existing cluster, you can update the application with the command 
below. Make sure to compare your existing `values.yaml` file to the [current one](http://packagist.com.lo/docs/self-hosted/kubernetes-helm#annotated-configuration) first.

``` 
helm upgrade -f values.yaml private-packagist oci://registry.replicated.com/privatepackagistkots/private-packagist --version VERSION
```

You can find the latest version in the [changelog](changelog). You can safely bump multiple versions at once, it's not required to
install individual versions consecutively. If we ever release a version that needs to be installed specifically, 
we will make sure to communicate this first. 

We recommend backing up your database before each update. We cannot guarantee that downgrading to a previous release will always work 
due to database migrations. In case of upgrade failure, the most reliable way to rollback to the previous version is via backup.
