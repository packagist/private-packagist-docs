# Installing Private Packagist Self-Hosted on an existing Kubernetes cluster
##

Private Packagist Self-Hosted leverages the [kots](https://docs.replicated.com/reference/kots-cli-getting-started)
kubectl plugin by Replicated to run on a Kubernetes cluster. The plugin provides a management interface to your
Private Packagist Self-Hosted installation. It also allows you to monitor the application and perform maintenance operations
such as backups or updates.

This page will guide you through an installation with an existing cluster, to instead install Private Packagist Self-Hosted
without an existing Kubernetes cluster follow [this guide](./kubernetes-embedded.md).

## General requirements

1. A Kubernetes cluster v1.23 or v1.24 
1. License Key File (file extension .yaml) Don't have one yet? [Sign up for a free trial license!](https://packagist.com/self-hosted)
1. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
1. One (sub-)domain to operate the Composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
1. An SSL certificate valid for both chosen domains or a way to generate one in your cluster like [cert-manager](https://cert-manager.io/)
1. An SMTP server or a GMail account for Private Packagist Self-Hosted to send email
1. If your firewall restricts external connections the following domains must be accessible from the server:
    * hub.docker.com
    * proxy.replicated.com
    * replicated.app
    * kots.io (required to install the kots CLI)
    * github.com (required to install the kots CLI)
<!-- See https://docs.replicated.com/enterprise/installing-general-requirements -->

## Installation

Private Packagist requires the installation of the [kots](https://docs.replicated.com/reference/kots-cli-getting-started)
kubectl plugin from Replicated. The plugin provides an admin console to configure and update Private Packagist Self-Hosted.

The commands below will install the kots plugin and add the Private Packagist Self-Hosted application.
At the end of the install script, kots will set up a port forward to localhost:8800 where you can continue with the Private
Packagist Self-Hosted setup. Once Private Packagist is fully configured and setup, it is recommended to stop the port
forwarding and only start it again to make changes to the configuration or update the application.

Please note: by default the install command will set up a MinIO instance for storage used by the admin console. You can
skip this by setting the `--with-minio` flag to `false`. For a full list of configuration options see the 
[CLI documentation](https://docs.replicated.com/reference/kots-cli-install).

```
curl https://kots.io/install | bash
# Verify the plugin installation
kubectl kots --help
# Install the Private Packagist application
kubectl kots install privatepackagistkots
```

To log in to the admin console, you will need the password shown at the end of the install command. You can also always
regenerate the admin console password via `sudo kubectl kots reset-password privatepackagistkots`.

### Replicated Configuration
#### Replicated Setup

Login to the admin console using the password generated during the kots application installation.

![Login to Admin Console](/Resources/public/img/docs/self-hosted-kubernetes/console-login.png)

On the next screen, you can upload the supplied .yaml license key file. If you don't have the license key file yet then
you can download it from https://packagist.com.

![Upload License](/Resources/public/img/docs/self-hosted-kubernetes/console-license.png)

#### Configure Private Packagist Self-Hosted
The configuration screen is where you can set up the domains used for Private Packagist and the email configuration. It
is also the place where you can configure if Private Packagist should use an existing Redis, PostgreSQL, or blob storage.
![Configuration](/Resources/public/img/docs/self-hosted-kubernetes/console-configure-application.png)

Every configuration change or application update will trigger a preflight check. Once the preflight check have passed,
the changes can be applied to your Kubernetes cluster.
![Preflight Check](/Resources/public/img/docs/self-hosted-kubernetes/console-preflight-check.png)

The application overview in the admin console shows you the application status, your current license information, and any
available updates for Private Packagist. Once the application has entered the ready state, you can access Private Packagist
via the domain configured for the web interface e.g. packagist.myintranet.com and finish the setup there.
![Application Overview](/Resources/public/img/docs/self-hosted-kubernetes/console-application-overview.png)

### Setup authentication and Select Admin

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
Configure the Storage Class under the Kubernetes Settings on the Config page in the admin console.

Alternatively, you can configure Private Packagist Self-Hosted to use your own PostgreSQL, Redis, and blob storage.
For blob storage, we currently support Azure Blob Storage, Google Cloud Storage, AWS S3, and other S3-compatible storage solutions.

## Security

The Private Packagist Self-Hosted application expects that TLS termination happens at or before the Ingress level.
All traffic within the cluster is unencrypted.

Make sure your Kubernetes network plugin encrypts connections between pods to avoid potential security issues.

## Backups

The Replicated admin console integrates with [Velero](https://velero.io/), a tool to back up and restore your Kubernetes
cluster resources and persistent volumes. Private Packagist Self-Hosted provides annotations to help back up and restore
the application with Velero.

If you are using your own backup solution, we recommend making regular backups of PostgreSQL, Redis, and the used blob
storage.
