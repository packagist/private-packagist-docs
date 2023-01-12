# Installing Private Packagist Self-Hosted on an existing Kubernetes cluster
##

Private Packagist Self-Hosted can run on a Kubernetes cluster. To do that, Private Packagist Self-Hosted uses Replicated.

Replicated is the application which will provide you with a management interface to your Private Packagist Self-Hosted installation
and allow you to monitor the task and perform maintenance operations such as backups or updates.

To install Private Packagist Self-Hosted without an existing Kubernetes cluster follow [this guide](./kubernetes-embedded.md).

## General requirements

1. License Key File (file extension .yaml) Don't have one yet? [Sign up for a free trial license!](https://packagist.com/self-hosted)
2. One (sub-)domain to operate the web interface, e.g. packagist.myintranet.com
3. One (sub-)domain to operate the Composer repository, e.g. repo.packagist.myintranet.com or packagist-repo.myintranet.com
4. An SSL certificate valid for both chosen domains or something like [cert-manager](https://cert-manager.io/) to generate a certificate
5. An SMTP server or a GMail account for Private Packagist Self-Hosted to send email
6. A Kubernetes v1.23 or v1.24 cluster
7. If your firewall restricts external connections the following domains must be accessible from the server:
    * hub.docker.com
    * proxy.replicated.com
    * replicated.app
    * kots.io (required to install the kots CLI)
    * github.com (required to install the kots CLI)
<!-- See https://docs.replicated.com/enterprise/installing-general-requirements -->

## Installation

Private Packagist requires the installation of the [kots](https://docs.replicated.com/reference/kots-cli-getting-started)
kubectl plugin from Replicated. The plugin provides an admin console to configure and update Private Packagist Self-Hosted.

The commands below will install the kots plugin and add the Private Packagist Self-Hosted application to your cluster.
At the end of the install script, kots will set up a port-forward to localhost:8800 where you can continue to setup
Private Packagist Self-Hosted. Once Private Packagist is fully set up it is recommended to stop the port forwarding and
only start it again to make changes to the configuration or update the application.

Please note: by default the install command will set up a MinIO instance for storage use by the admin console, to skip this
set this `--with-minio` flag to `false`. For a full list of configuration options see [CLI documentation](https://docs.replicated.com/reference/kots-cli-install).

```
curl https://kots.io/install | bash
# Verify the plugin installation
kubectl kots --help
# Install the Private Packagist application
kubectl kots install privatepackagistkots
```

To login to the admin console you will need the password shown at the end of the install command. You can also always
regenerate the admin console password via `sudo kubectl kots reset-password privatepackagistkots`.

### Login to Admin Console and Configure Private Packagist Self-Hosted

Login to the admin console using the password generated during the kots application installation.

![Login to Admin Console](/Resources/public/img/docs/self-hosted-kubernetes/console-login.png)

On the next screen you can upload the supplied .yaml license key file. If you don't have the license key file yet then
you can download it from https://packagist.com.

![Upload License](/Resources/public/img/docs/self-hosted-kubernetes/console-license.png)

The configuration screen is where you can setup the domains used for Private Packagist and the email configuration. It
is also the place where you can configure if Private Packagist should use an existing Redis, PostgreSQL, or blob storage.
![Upload License](/Resources/public/img/docs/self-hosted-kubernetes/console-config.png)

Every configuration change or application update will trigger a preflight check. Once the preflight check passed, the changes
can be applied to your Kubernetes cluster.
![Preflight Check](/Resources/public/img/docs/self-hosted-kubernetes/console-preflight.png)

The application overview in the admin console shows you the application status, your current license information, and any
available updates for Private Packagist. Once the application has entered the ready state, you can access Private Packagist
via the domain configured for the web interface e.g. packagist.myintranet.com and finish the setup there.
![Application Overview](/Resources/public/img/docs/self-hosted-kubernetes/console-application-overview.png)

### Setup authenticateion and Select Admin

#### Authentication Setup
Within Private Packagist Self-Hosted you now need to set up at least one user authentication method. You have the choice between authentication with email addresses and passwords and several OAuth integrations with third party services.
We provide integrations with on-premises versions of GitHub, Bitbucket, or GitLab, or their public services on github.com, bitbucket.org or gitlab.com. Follow the instructions to create the respective required identifiers, tokens and secrets.

* [GitHub (Enterprise) Integration Setup](./github-integration-setup.md)
* [Bitbucket.org Integration Setup](./bitbucket-integration-setup.md)
* [Bitbucket Data Center / Server Integration Setup](./bitbucket-server-integration-setup.md)
* [GitLab Integration Setup](./gitlab-integration-setup.md)
* [Authentication with Email Addresses and Passwords](./authentication-email-addresses-passwords-setup.md).


![Add Integration](/Resources/public/img/docs/self-hosted/08-integration.png)

#### Selecting Admins
After setting up an integration you can login through the top menu. Register an account and pick a username.

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
Configure the Storage Class under the Kubernetes Settings on Config page in the admin console

Alternately, you can configure Private Packagist Self-Hosted to use your own PostgreSQL, Redis, and blob storage.
For blob storage, we currently support Azure Blob Storage, Google Cloud Storage, AWS S3, and other S3-compatible storage solutions.

## Security

The Private Packagist Self-Hosted application expects that TLS termination happens at or before the Ingress level.
All traffic within the cluster is unencrypted.

Make sure your Kubernetes network plugin encrypts connections between pods to avoid potential security issues.

## Backups

The Replicated Admin console integrates with [Velero](https://velero.io/), a tool to back up and restore your Kubernetes
cluster resources and persistent volumes and Private Packagist Self-Hosted provides annotations to help back up and restore
the application with Velero.

If you are using your own backup solution we recommend to make regular backups of PostgreSQL, Redis, and the used blob
storage.
