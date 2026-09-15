# Private Packagist Self-Hosted
##

Private Packagist Self-Hosted Kubernetes is the next iteration of our Self-Hosted product, allowing you to run
Private Packagist in your own data center.

### How it works
Private Packagist Self-Hosted runs on a Kubernetes cluster and is distributed by [Replicated](https://www.replicated.com/).
It can either be installed with Helm in an existing cluster or using an installer that creates an embedded Kubernetes
cluster on a linux machine.

#### Installing the Private Packagist Self-Hosted Kubernetes Helm chart in an existing cluster

If you already have an existing Kubernetes cluster running, and are comfortable installing Helm charts then follow [this guide](./kubernetes-helm.md).
You configure the installation in a `values.yaml` file and apply updates with `helm upgrade`.

#### Installing Private Packagist Self-Hosted Kubernetes in a Kubernetes cluster installed with kURL

Don't use Kubernetes? Use the installer that takes care of installing Private Packagist Self-Hosted on a linux
machine following [this guide](./kubernetes-embedded.md). This installation uses the
[kots](https://docs.replicated.com/reference/kots-cli-getting-started) kubectl plugin by Replicated. The plugin
provides a management interface to your Private Packagist Self-Hosted installation and allows you to monitor the
application and perform maintenance operations such as backups or updates.
