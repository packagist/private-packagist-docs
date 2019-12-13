# Troubleshooting
## Private Packagist

### New versions do not appear immediately

The amount of time between pushing a new version and seeing it on Private Packagist depends on how you added the package to Private Packagist. The following sections explain each scenario. If a version does not appear within the listed time, check the update log for problems. The update log can be found on the package page via "View Log". The update log will contain information about versions which could not be loaded into Private Packagist, e.g. the composer.json file is invalid.

##### Packages mirrored from packagist.org

New or updated versions for packages which are mirrored from packagist.org appear within seconds after the versions are available on packagist.org. The time between pushing to the VCS repository of packagist.org package and a change appearing on Private Packagist depends on whether the package has a working webhook set up on packagist.org and on how long it takes for GitHub/GitLab/Bitbucket to notify packagist.org about the change.

##### Packages added to Private Packagist by URL either with a credential or via synchronization

If you add a package by URL or through synchronization from GitHub, GitLab, or Bitbucket, we attempt to set up a webhook on the repository. This only works if you use synchronization or if you specified credentials to access the API along with the URL. We cannot set up webhooks for public packages without credentials to access their settings through the API.
A successfully set up webhook means that new versions will appear within seconds or a few minutes after you push to the Git repository.

##### All other packages

For all other packages Private Packagist does not get notified about new versions and Private Packagist checks for updates at least once every 12 hours. Thus, new versions of these other packages will appear on Private Packagist with a delay of up to 12 hours.

Other packages include:
- VCS repositories to which we have been granted access via SSH key
- repositories which are hosted on other platforms than GitHub, GitLab or Bitbucket
- packages added from third party mirrored repositories other than packagist.org
- repositories that have been added without a credential
