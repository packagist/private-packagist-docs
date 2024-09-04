# Installing the Private Packagist Self-Hosted Helm chart in an existing Kubernetes cluster
##

The Private Packagist Self-Hosted Helm chart allows you to install Private Packagist in an existing Kubernetes cluster,
to instead install Private Packagist Self-Hosted without an existing Kubernetes cluster, follow [this guide](./kubernetes-embedded.md).

## General requirements

1. A Kubernetes cluster v1.23 or newer
1. Your username and password to log in to the Helm registry on registry.replicated.com. You can find your credentials in your Private Packagist account at https://packagist.com. Don't have one yet? [Sign up for a free trial license!](https://packagist.com/self-hosted)
1. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
1. One (sub-)domain to operate the composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
1. An SSL certificate valid for both chosen domains
1. An SMTP server or a GMail account for Private Packagist Self-Hosted to send email
1. If your firewall restricts external connections then the following domains must be accessible from the server:
  * hub.docker.com
  * proxy.replicated.com
  * registry.replicated.com
  * replicated.app
  * amazonaws.com
  * k8s.gcr.io

## Installation

Private Packagist Self-Hosted requires PostgreSQL, Redis, and blob storage to store application data and Composer packages.
You can either use the built-in options that come with the Helm chart or use your own PostgreSQL, Redis, and blob storage.
For blob storage, we currently support Azure Blob Storage, Google Cloud Storage, AWS S3, and other S3-compatible storage solutions.

Please note that if you choose to use the built-in solution then each of the storage requires one or more volumes using
[dynamic volume provision](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) to allocate storage for the different Pods.
Configure the Storage Class in the values.yaml under `global.storageClass`.

### Annotated configuration

To install the Private Packagist Self-Hosted Helm Chart configure values based on your setup, store them in a YAML file, e.g. `values.yaml`, and then run the commands below.

HELM_CHART_VALUES_FILE

#### Login to the Helm registry and install the chart

Make sure you replace `YOUR_USERNAME`, `YOUR_PASSWORD`, `values.yaml`, and `VERSION` with your values before running the commands.
You can find the latest Private Packagist version on our [changelog page](/docs/self-hosted/changelog).

```
helm registry login registry.replicated.com --username YOUR_USERNAME --password YOUR_PASSWORD
helm install -f values.yaml private-packagist oci://registry.replicated.com/privatepackagistkots/private-packagist --version VERSION
```

#### Authentication Setup
Within Private Packagist Self-Hosted, you now need to set up at least one user authentication method.
You have the choice between authentication with email addresses and passwords and several OAuth integrations with third-party services.
We provide integrations with on-premises versions of GitHub, Bitbucket, GitLab, or their public services on github.com, bitbucket.org,
or gitlab.com. Follow the instructions to create the respective required identifiers, tokens, and secrets.

* [GitHub (Enterprise) Integration Setup](./github-integration-setup.md)
* [Bitbucket.org Integration Setup](./bitbucket-integration-setup.md)
* [Bitbucket Data Center / Server Integration Setup](./bitbucket-server-integration-setup.md)
* [GitLab Integration Setup](./gitlab-integration-setup.md)
* [Authentication with Email Addresses and Passwords](./authentication-email-addresses-passwords-setup.md).


![Add Integration](/Resources/public/img/docs/self-hosted/08-integration.png)

#### Selecting Admins
After setting up an integration, you can log in through the top menu. Register an account and pick a username.

![Register Admin](/Resources/public/img/docs/self-hosted/09-register-admin.png)

The first user is granted admin permissions automatically. You can grant admin permissions to more users in the admin panel.

![Add Admin](/Resources/public/img/docs/self-hosted/10-add-admin.png)

#### Switching to Production Mode
Head back to the admin console to disable the Setup Mode in the configuration. Once the preflight checks passed, you can
apply the changes.

That’s it! Private Packagist Self-Hosted is now ready to be used! You’ll find all further information in the web interface.

## Database and storage with dynamic volumes

Private Packagist Self-Hosted will set up PostgreSQL, Redis, and MinIO to store application data and Composer packages.
Each of them requires one or more volumes using [dynamic volume provision](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) to allocate storage for the different Pods.
Configure the Storage Class in the values.yaml under `global.storageClass`.

Alternatively, you can configure Private Packagist Self-Hosted to use your own PostgreSQL, Redis, and blob storage.
For blob storage, we currently support Azure Blob Storage, Google Cloud Storage, AWS S3, and other S3-compatible storage solutions.
You can configure the blog storage in the values.yaml under `storage.type`.

## Security

The Private Packagist Self-Hosted application expects that TLS termination happens at or before the Ingress level.
All traffic within the cluster is unencrypted.

Ensure your Kubernetes network plugin encrypts connections between pods to avoid potential security issues.

## Backups

The Private Packagist Self-Hosted application integrates with [Velero](https://velero.io/), a tool to back up and restore your Kubernetes
cluster resources and persistent volumes. Private Packagist Self-Hosted provides annotations to help back up and restore
the application with Velero.

If you are using your own backup solution, we recommend making regular backups of PostgreSQL, Redis, and the used blob
storage.
