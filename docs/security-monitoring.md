# Security Monitoring

Private Packagist Security Monitoring searches the dependencies of your projects for known security vulnerabilities.
Projects, which are Composer packages with a composer.lock file, are analyzed every time you push a new commit to your project and when a new vulnerability is published in one of the databases.

The following databases are used to analyze your projects:
* [FriendsOfPHP/security-advisories](https://github.com/FriendsOfPHP/security-advisories)

<div class="row column">
    <div class="callout warning">
        <p>Security monitoring is available for packages in your organization. To monitor your projects, you must add them to Private Packagist as packages, even if you do not intend to install them as a dependency.</p>
    </div>
</div>


### Configuring Monitoring Settings
The default branch of every project is automatically monitored if a composer.lock file is present.
Additional branches to be monitored can be selected on the package page.

Security monitoring can be disabled for individual project packages on the package security page or for all projects
on the organization’s security settings page.
Organizations using subrepositories have the option to disable security monitoring for individual subrepositories.

### Configuring Notifications
Every user receives security notifications by email for all projects they have access to by default.
Users can unsubscribe either from individual projects or from all security notifications if they do not wish to receive any email notifications.

[Notification channels](./notifications.md) allow you to receive security notifications via email to other addresses, on Slack, on Microsoft Teams or through a webhook to an endpoint of your choice. They can be configured on your organization’s settings page under *Notification Channels -> Add Notification Channel*.

Once you create a notification channel, you can assign it to the packages to be monitored on the organization's security settings.

### Resolving Security Issues
When Private Packagist finds a security issue it will list safe versions, which you can update to, in order to resolve the problem.
Once your project has been updated to use a secure version of the dependency the issue will automatically be closed.
You can also manually close an issue for instance in the case where the affected component is not actually used in your project.

![Handle security issues](/Resources/public/img/docs/features/Package-SecurityMonitoring-20200723.png)
