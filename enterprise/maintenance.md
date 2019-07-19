# Maintenance
## Private Packagist Enterprise

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

### Private Packagist Enterprise CLI

Private Packagist Enterprise installs a command line tool `packagist` on the host system which provides access to some administrative features via command line.

#### Updating all packages

Run the command `packagist package-update-all [--overwrite-data=true|false] [-only-non-updated=true|false]` to schedule update jobs on the worker for all packages on your installation. The flag `--overwrite-data` ensures that all existing version data is downloaded from the source again and version data is overwritten if it has changed. The flag `--only-non-updated` limits the set of packages to those which have only been initialized but never updated yet.
