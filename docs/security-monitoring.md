# Security Monitoring

Private Packagist Security Monitoring searches the dependencies of your private packages for known security vulnerabilities.
Packages are analyzed every time we find a new commit in your package and when a new vulnerability is published in one of the databases.

The following databases are used to analyze your packages:
* [FriendsOfPHP/security-advisories](https://github.com/FriendsOfPHP/security-advisories)

<div class="row column">
    <div class="callout warning">
        <p>Security monitoring is available for packages in your organization. To monitor your projects, you must add them to Private Packagist as packages, even if you do not intend to install them as a dependency.</p>
    </div>
</div>


### Configuring Monitoring Settings
The default branch of every private package is automatically monitored if a composer.lock file is present.
Additional branches to be monitored can be selected on the package page.

Security monitoring can be disabled for individual packages on the package security page or for all packages
on the organization’s security settings page.
Organizations using the agency add-on have the option to disable security monitoring for individual subrepositories.

### Configuring Notifications
Every user receives security notifications by email for all packages they have access to by default.
Users can unsubscribe either from individual packages or from all security notifications if they do not wish to receive any email notifications.

The following notification channels are supported:
* Email: send notifications to a configured email addresses list
* Slack webhook: receive notifications in your configured Slack channel
* Webhook: create a webhook with your own URL and a secret in Private Packagist. You can validate the secret using [our api client](https://github.com/packagist/private-packagist-api-client#validate-incoming-webhook-payloads) or by running ```hash_equals('sha1='.hash_hmac('sha1', (string) $request->getBody(), $SECRET_USER_CHOSEN), $response->getHeader('Packagist-Signature'));```

Notification channels can be added on your organisation’s settings page under *Notification Channels -> Add Notification Channel*.

### Resolving Security Issues
When Private Packagist finds a security issue it will list safe versions, which you can update to, in order to resolve the problem.
Once your project has been updated to use a secure version of the dependency the issue will automatically be closed.
You can also manually close an issue for instance in the case where the affected component is not actually used in your project.

![Handle security issues](/Resources/public/img/docs/feature/security-monitoring-handle-issues.png)
