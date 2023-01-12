## Installing Private Packagist Self-Hosted
##

Private Packagist Self-Hosted can run on a Kubernetes cluster. To do that, Private Packagist Self-Hosted uses Replicated.

Replicated is the application which will provide you with a management interface to your Private Packagist Self-Hosted installation
and allow you to monitor the task and perform maintenance operations such as backups or updates.

To install Private Packagist Self-Hosted in an existing Kubernetes cluster instead follow [this guide](./kubernetes-existing.md).

## General requirements

1. License Key File (file extension .yaml) Don't have one yet? [Sign up for a free trial license!](https://packagist.com/self-hosted)
2. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
3. One (sub-)domain to operate the composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
4. An SSL certificate valid for both chosen domains or use Let's Encrypt to generate a certificate for you
5. An SMTP server or a GMail account for Private Packagist Self-Hosted to send email
6. If your firewall restricts external connections the following domains must be accessible from the server:
  * hub.docker.com
  * proxy.replicated.com
  * replicated.app
  * kots.io (required to install the kots CLI)
  * github.com (required to install the kots CLI)
<!-- See https://docs.replicated.com/enterprise/installing-general-requirements -->

## Installation

To install Private Packagist Self-Hosted and Replicated run following command:
```
curl -sSL https://kurl.sh/privatepackagistkots | sudo bash
```

To learn more about options for the easy install script, please refer to the Replicated manual on Installing Replicated at [https://help.replicated.com/docs/kubernetes/customer-installations/installing/](https://help.replicated.com/docs/kubernetes/customer-installations/installing/).

After your Replicated Kubernetes is up and running you can follow the rest of the Packagist guide.

## Database and storage

Private Packagist Self-Hosted will set up PostgreSQL, Redis, and MinIO to store application data and Composer packages.
Each of them requires one or more volumes if you prefer to avoid that then you can configure to use your own PostgreSQL,
Redis, and blob storage. For blob storage, we currently support Azure Blob Storage, Google Cloud Storage, AWS S3, and
other S3-compatible storage solutions.

Please note that the backup solution provided only covers the built-in services and you are responsible for creating backups
of any services that you run yourself.

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