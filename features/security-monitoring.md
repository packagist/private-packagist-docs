# Security Monitoring
The Private Packagist security monitoring feature analyzes the dependencies of your private packages for known security vulnerabilities.
Packages get analyzed on every commit and every time a database reports new vulnerabilities.

The following databases are used to analyze your packages:
* [sensiolabs/security-advisories](https://github.com/FriendsOfPHP/security-advisories)

### Configure monitoring settings
The default branch of every private package gets automatically monitored if a composer.lock file is present.
Additional branches can be configured on the package page.

The feature can be disabled for individual packages on the package security page or for all packages
on the organization’s security settings page.
Organizations using the agency add-on also have the option to disable the feature for individual subrepositories.

### Configure notifications
Every user receives security notifications for all packages they have access to by default.
They can unsubscribe from individual packages or from the feature all together if they do not with to receive any emails.

Additional notification channels to receive emails at an address not belonging to a user or to get notified via slack
can be created on the notification channel settings page and configured on the organisation’s security settings.

### Handle security issues
For every found security issue Private Packagist will show you which versions of the dependency resolve the issue.
Once your project has been updated to use a secure version of the dependency the issue will automatically be closed.
You can also manually close an issue for instance in the case where the affected component is not actually used in your project.

![Handle security issues](/Resources/public/img/docs/feature/security-monitoring-handle-issues.png)
