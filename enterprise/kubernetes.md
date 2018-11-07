# Installing Private Packagist Enterprise on Kubernetes

Private Packagist Enterprise can run on a Kubernetes cluster. To do that, Private Packagist Enterprise uses Replicated.

Replicated is the application which will provide you with a management interface to your Private Packagist Enterprise installation and allow you to monitor the task and perform maintenance operations such as backups or updates.

## General requirements

1. License Key File (file extension .rli) Don't have one yet? [Sign up for a free trial license!](https://packagist.com/enterprise)
2. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
3. One (sub-)domain to operate the composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
4. An SSL certificate valid for both chosen domains
5. An SMTP server or a GMail account for Private Packagist Enterprise to send email

## Options

There are two options for running Private Packagist Enterprise on a Kubernetes cluster:

* **Start from scratch.** Installing Replicated Kubernetes on a Linux server and running Private Packagist Enterprise there
* **Use an existing Kubernetes cluster.** Installing Replicated on your existing Kubernetes cluster and running Private Packagist Enterprise there

## Installing Private Packagist Enterprise from scratch

### Requirements

* A Linux Server
  * At least 4GB memory
  * At least 2 CPU cores
  * At least 40GB disk space
  * Ports 443 and 8800 must be accessible
  * Must be reachable at the chosen domain names from your local machine

### Installation

The easiest way to install Replicated Kubernetes is the easy install script. You can run it using the following two commands:

```
curl -sSL -o install.sh https://get.replicated.com/privatepackagistkubernetes/stable/kubernetes-init
sudo bash ./install.sh
```

Alternatively you can [install Replicated onto a machine that is not connected to the internet](https://help.replicated.com/docs/kubernetes/customer-installations/airgapped-installations/).

To learn about options for the easy install script, please refer to the Replicated manual on Installing Replicated at https://help.replicated.com/docs/kubernetes/customer-installations/installing/.

After your Replicated Kubernetes is up and running you can follow the rest of the Packagist guide.

## Installing Private Packagist Enterprise on an existing Kubernetes cluster

### Requirements

* A Kubernetes v1.11.x cluster

### Installation

If you already have a Kubernetes cluster, you can install Replicated there and use it to install Private Packagist Enterprise.

To do that you can use the `https://get.replicated.com/kubernetes-yml-generate` script to generate a yaml file including specs for all the Deployments, Services, and PersistentVolumeClaims required by Replicated.

Replicated and Private Packagist Enterprise use [dynamic volume provision](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) to allocate storage for the different Pods, so your cluster should have a provisioner with one or more Storage Classes defined.
Please refer to the documentation for more information about this topic.

Once you know the Storage Class you want to use for your installation, you can use the `storage_class` parameter (and disable the Host Path provisioner) to get your Replicated yaml file and apply it to your Kubernetes cluster:

```
curl -sSL "https://get.replicated.com/kubernetes-yml-generate?storage_class=standard&storage_provisioner=0" | bash > replicated.yml
kubectl apply -f replicated.yml
```

After replicated is installed you can follow the rest of the Packagist guide, making sure you configure the same Storage Class in the Packagist configuration too.

### Security

The Private Packagist Enterprise application terminates TLS at the Ingress level, so all traffic within the cluster is unencrypted.

Make sure your Kubernetes network plugin encrypts connections between pods to avoid potential security issues.

### GKE quirks

When running Private Packagist Enterprise on Google Kubernetes Engine, there are some quirks that have to be taken into account.

#### Replicated

When the Replicated yaml is generated, it exposes a `NodePort` service for the Replicated UI, you might want to change it to a `LoadBalancer` so you can access the setup URL.

After applying the Replicated yaml, you might see that some of the `retraced-` pods are marked as `Pending`. This is caused by Pod affinity rules, simply edit the corresponding deployments and remove the affinity rules to make it work.

#### Packagist

The Load Balancer for the minio Ingress endpoint won't be properly configured because there's no readiness probe in the minio Stateful Set, see https://github.com/minio/minio/pull/5650 and https://github.com/kubernetes/kubernetes/issues/27114 for details.

To fix it, you need to configure the Health Check for the minio Backend service to point to the `/minio/health/live` path. To access the Health Check configuration you need to go to the `web-front` service in the GKE console, select the minio Backend service, and then edit the Health check.

## Minio

Private Packagist Enterprise sets up a minio instance in the cluster to store the composer packages. You can access the minio instance on path `/minio` under your chosen domain, the credentials can be found in the `minio-secret-key` Kubernetes Secret.

## Known issues

Making configuration changes while the application is running requires that the relevant pods are restarted for the changes to be applied.

For example, when disabling "Setup Mode" after installing Private Packagist Enterprise the `ui` and `repo` pod need to be restarted for changes to be applied.

This is so because the configuration lives in a ConfigMap and signaling pods for ConfigMap updates is a feature still in the works. See https://github.com/kubernetes/kubernetes/issues/22368 for more details.
