# Maintenance
## Private Packagist Self-Hosted (Kubernetes)

## Updates

### Updating kURL installations

This section describes how to update a Private Packagist Self-Hosted in a Kubernetes cluster installed with [kURL](kubernetes-embedded).
This section is not relevant if you installed the application with [Helm](kubernetes-helm).

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

### Updating Helm installations 

If you installed Private Packagist Self-Hosted with Helm into your existing cluster, you can update the application with this command:

``` 
helm upgrade -f values.yaml private-packagist oci://registry.replicated.com/privatepackagistkots/private-packagist --version VERSION
```

You can find the latest version in the [changelog](changelog). Make sure to compare your existing `values.yaml` file to the [current one](http://packagist.com.lo/docs/self-hosted/kubernetes-helm#annotated-configuration).  
