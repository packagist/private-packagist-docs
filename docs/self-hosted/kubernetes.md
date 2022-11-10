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

<!-- See https://kurl.sh/docs/install-with-kurl/system-requirements#networking-requirements and https://docs.replicated.com/enterprise/installing-general-requirements -->
* If your firewall restricts external connections the following domains must be accessible from the server:
  * amazonaws.com
  * k8s.gcr.io
  * k8s.kurl.sh
  * hub.docker.com/
  * proxy.replicated.com
  * replicated.app

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

<!-- See https://docs.replicated.com/enterprise/installing-general-requirements -->
* If your firewall restricts external connections the following domains must be accessible from the server:
  * hub.docker.com/
  * proxy.replicated.com
  * replicated.app
  * kots.io (required to install the kots CLI)
  * github.com (required to install the kots CLI)

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

The Replicated Admin console integrates with [Velero](https://velero.io/), a tool to back up and restore your Kubernetes
cluster resources and persistent volumes. It is automatically installed with an embedded cluster installation.

Once your Private Packagist Self-Hosted is up and running, you can configure the storage destination and the backup
schedule in the Replicated Admin console under Snapshot settings. We recommend using an external storage solution like
Amazon S3 and configuring full snapshots.

To restore Private Packagist Self-Hosted from a snapshot, access the "Full Snapshots" and click on the "Restore from backup"
icon. You will then see information on how to either perform a full restore or only restore the Private Packagist Self-Hosted
application. During the restore process both Private Packagist Self-Hosted and the Replicated Admin console will become
unavailable.

If you are installing Private Packagist Self-Hosted into an existing Kubernetes cluster, then you can either install
[Velero](https://velero.io/), and configure it as described above or create backups using your solution.
