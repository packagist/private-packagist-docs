# Maintenance
## Private Packagist Self-Hosted (Replicated Native)

This guide explains how to maintain the now deprecated Private Packagist Self-Hosted (Replicated Native).  
If you are using our product Private Packagist Self-Hosted Kubernetes, [use this guide](./kubernetes-maintenance.md).

<div class="row column">
    <div class="callout warning">
        <p>Replicated Native shuts down November 30th, 2024.</p>
        <p>
            To keep using Private Packagist Self-Hosted, you need to migrate to the next iteration of our Self-Hosted product: <a href="/docs/self-hosted/kubernetes">Private Packagist Self-Hosted Kubernetes</a>.        
        </p>
    </div>
</div>

### Updates
By default Replicated checks for updates every 5 hours and you can install them from the dashboard once they are available. You can configure this behaviour under “Console Settings” when clicking on the gear icon in the management console.

The [Private Packagist Self-Hosted Changelog](https://packagist.com/docs/self-hosted/changelog) details all new features, behavior changes and major bugfixes each release introduces.

If your firewall restricts external connections the following domains must be accessible from the server:

REPLICATED_DOMAIN_LIST
   
### Updating or Replacing the SSL Certificate
If you would like to upload a new SSL certificate or if you would like to reload an existing SSL certification from the host filesystem please open the Replicated Management Console at port 8800.
Then open the settings menu with the gear icon on the top right and select "Console Settings". Scroll to "TLS Key & Cert" or click on the respective option in the menu. Here you can select a new certificate.
Save the page to apply the new certificate to the Replicated Management Console. If you are using a server path and replaced the SSL certificate without changing the path, save the page to reload the certificate.

Go back to the dashboard on the Replicated Management Console and restart the application for the SSL certificate to be applied to the whole Private Packagist application.

If your certificate requires intermediate certificates to be recognized by your browser and/or Composer you can paste them into your certificate text file. Please make sure they are sorted from leaf (your certificate) via intermediate certificate to root certificate at the bottom. Otherwise Replicated will not recognize the certificate in a combined file.

Please see the [Troubleshooting](./troubleshooting.md) page for details on dealing with SSL errors.

#### Automating the replacement of the SSL certificate

You can automate the reloading of an SSL certificate and restarting the application for the new certificate to be used, for example if you use letsencrypt using the following commands:

```
replicated console cert set hostname.goes.here /path/to/key /path/to/cert
replicatedctl app apply-config
```

### Private Packagist Self-Hosted CLI

Private Packagist Self-Hosted installs a command line tool `packagist` on the host system which provides access to some administrative features via command line.

#### Updating all packages

Run the command `packagist package-update-all [--overwrite-data=true|false] [-only-non-updated=true|false]` to schedule update jobs on the worker for all packages on your installation. The flag `--overwrite-data` ensures that all existing version data is downloaded from the source again and version data is overwritten if it has changed. The flag `--only-non-updated` limits the set of packages to those which have only been initialized but never updated yet.

### Migrate Private Packagist to another server

Create a snapshot either via the Replicated admin console or on the CLI with `replicatedctl snapshot start` and either transfer the snapshot to the new machine or make sure it is accessible from the new machine. We recommend to use SFTP on a remote machine or S3 to store snapshots, which can then also be accessed from a new server.
On the new machine follow the [Installation Guide](./installation.md) up to the point where you are asked to upload your license.
Select "Restore from a snapshot" instead of "Choose license", enter the full path to your snapshot folder, and select the snapshot you want to restore from.
