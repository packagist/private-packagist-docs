# Troubleshooting
## Private Packagist

### New versions do not appear instant

Depending on how the package has been added to Private Packagist it may take a different amount of time until
new or updated versions appear in Private Packagist. The times of how long it can take for new versions to appear
in Private Packagist can be found in the section below. In case the version does not appear within the mentioned time frame
the update log which can be accessed on the package page via "View Log" can contain information about why a certain versions
was not picked up by Private Packagist e.g. the composer.json file is invalid.

##### Packages added from packagist.org

New or updated versions for packages that are mirrored from packagist.org appear within seconds or minutes 
after the update is available on packagist.org. The time from push to the VCS repository until it appears on Private Packagist
depends on whether the package has a working webhook set up on packagist.org and how long it takes for GitHub/GitLab/Bitbucket
to notify packagist.org.

##### Packages added to Private Packagist either with a credential or via synchronization

For packages which are imported from GitHub/GitLab/Bitbucket where we have access to the respective API with credentials
there we attempt to set up webhooks to get notified about new versions. This means that new versions will appear within
seconds or a few minutes after the VCS repository push.

##### All other packages

For all other packages, this includes repositories to which we have been granted via ssh key, repositories that are hosted on
other platforms than GitHub, GitLab and Bitbucket, packages added from a third party mirrored repository other than packagist.org
and repositories that have been added without a credential, Private Packagist does not get notified about new versions. Therefore
we run updates on regular intervals once every three hours. This means new or updated versions will not appear instant on Private Packagist. 
