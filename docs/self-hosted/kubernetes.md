# Private Packagist Self-Hosted
##

Private Packagist Self-Hosted Kubernetes is the next iteration of our Self-Hosted product, allowing you to run
Private Packagist in your own data center.

### How it works
Private Packagist Self-Hosted leverages the [kots](https://docs.replicated.com/reference/kots-cli-getting-started)
kubectl plugin by Replicated to run on a Kubernetes cluster. The plugin provides a management interface to your
Private Packagist Self-Hosted installation and allows you to monitor the application and perform maintenance operations
such as backups or updates.

Private Packagist Self-Hosted Kubernetes can either be installed in an existing cluster or using an installer that creates
an embedded Kubernetes cluster on a linux machine.

#### Installing Private Packagist Self-Hosted Kubernetes in an existing cluster

If you already have an existing Kubernetes cluster running, follow [this guide](./kubernetes-existing.md).

#### Installing Private Packagist Self-Hosted Kubernetes in a Kubernetes cluster installed with kURL

Don't use Kubernetes? Use the installer that takes care of installing Private Packagist Self-Hosted on a linux
machine following [this guide](./kubernetes-embedded.md).
