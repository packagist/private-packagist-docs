# Installing Private Packagist Self-Hosted on Kubernetes

Private Packagist Self-Hosted can run on a Kubernetes cluster. To do that, Private Packagist Self-Hosted uses Replicated.

Replicated is the application which will provide you with a management interface to your Private Packagist Self-Hosted installation and allow you to monitor the task and perform maintenance operations such as backups or updates.

## General requirements

1. License Key File (file extension .yaml) Don't have one yet? [Sign up for a free trial license!](https://packagist.com/self-hosted)
2. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
3. One (sub-)domain to operate the composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
4. An SSL certificate valid for both chosen domains
5. An SMTP server or a GMail account for Private Packagist Self-Hosted to send email

## Options

There are two options for running Private Packagist Self-Hosted on a Kubernetes cluster:

* **Start from scratch.** Installing Replicated Kubernetes on a Linux server and running Private Packagist Self-Hosted there
* **Use an existing Kubernetes cluster.** Installing Replicated on your existing Kubernetes cluster and running Private Packagist Self-Hosted there

## Installing Private Packagist Self-Hosted from scratch

### Requirements

<!-- See https://kurl.sh/docs/install-with-kurl/system-requirements -->
* A Linux Server
  * At least 8GB memory
  * At least 4 CPU cores
  * At least 40GB disk space
  * Ports 443 and 8800 must be accessible
  * Must be reachable at the chosen domain names from your local machine

<!-- See https://kurl.sh/docs/install-with-kurl/system-requirements#networking-requirements -->
* If your firewall restricts external connections the following domains must be accessible from the server:
  * amazonaws.com
  * k8s.gcr.io
  * k8s.kurl.sh

### Installation

The easiest way to install Replicated is the easy install script. You can run it using the following command:

```
curl -sSL https://kurl.sh/privatepackagistkots | sudo bash
```

To learn more about options for the easy install script, please refer to the Replicated manual on Installing Replicated at https://help.replicated.com/docs/kubernetes/customer-installations/installing/.

After your Replicated Kubernetes is up and running you can follow the rest of the Packagist guide.

## Installing Private Packagist Self-Hosted on an existing Kubernetes cluster

### Requirements

* A Kubernetes v1.23.x cluster

### Installation

If you already have a Kubernetes cluster, you can install Replicated there and use it to install Private Packagist Enterprise.

```
curl https://kots.io/install | bash
kubectl kots install privatepackagistkots
```

Replicated and Private Packagist Self-Hosted use [dynamic volume provision](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) to allocate storage for the different Pods, so your cluster should have a provisioner with one or more Storage Classes defined.
Please refer to the documentation for more information about this topic.

Once you know the Storage Class you want to use for your installation, you can use the `storage_class` parameter (and disable the Host Path provisioner) to get your Replicated yaml file and apply it to your Kubernetes cluster:

After replicated is installed you can follow the rest of the Packagist guide, making sure you configure the same Storage Class in the Packagist configuration too.

### Security

The Private Packagist Self-Hosted application terminates TLS at the Ingress level, so all traffic within the cluster is unencrypted.

Make sure your Kubernetes network plugin encrypts connections between pods to avoid potential security issues.

### GKE quirks

When running Private Packagist Self-Hosted on Google Kubernetes Engine, there are some quirks that have to be taken into account.

#### Replicated

When the Replicated yaml is generated, it exposes a `NodePort` service for the Replicated UI, you might want to change it to a `LoadBalancer` so you can access the setup URL.

After applying the Replicated yaml, you might see that some of the `retraced-` pods are marked as `Pending`. This is caused by Pod affinity rules, simply edit the corresponding deployments and remove the affinity rules to make it work.

#### Packagist

The Load Balancer for the minio Ingress endpoint won't be properly configured because there's no readiness probe in the minio Stateful Set, see https://github.com/minio/minio/pull/5650 and https://github.com/kubernetes/kubernetes/issues/27114 for details.

To fix it, you need to configure the Health Check for the minio Backend service to point to the `/minio/health/live` path. To access the Health Check configuration you need to go to the `web-front` service in the GKE console, select the minio Backend service, and then edit the Health check.

## Minio

Private Packagist Self-Hosted sets up a minio instance in the cluster to store Composer packages. You can access the minio instance on path `/minio` under your chosen domain, the credentials can be found in the `minio-secret-key` Kubernetes Secret.

## Backups

Installations with an embedded cluster come with [Velero](https://velero.io/), a backup solution that allows you to configure
backups for either the entire setup or only the Private Packagist application. You can configure the snapshot frequency
as well as the storage location in the Replicated admin UI under Snapshots once everything is up and running.

If you are installing Private Packagist Self-Hosted into an existing Kubernetes cluster then creating backups of the
used embedded PostgreSQL and Redis database, as well as the Min.io storage is your responsibility.

## Configuration changes

Making changes to the Private Packagist configuration like disabling the "Setup Mode" or changing the email settings requires
a new deployment and restart several pods. This is so because the configuration lives in a ConfigMap and signaling pods
for ConfigMap updates is a feature still in the works. See https://github.com/kubernetes/kubernetes/issues/22368 for more details.
