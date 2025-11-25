# Notifications

### Notification Types

#### Package Releases

Private Packagist will notify you every time a new or modified version is discovered in one of the selected packages, e.g. anytime a tag, branch or commit is created or modified in a VCS repository.
You can specify which private packages you want to get notifications for and whether you want to receive notifications for mirrored packages.
You can filter releases by stability: "dev / any" will match any release including commits to a branch, whereas "stable" will only match releases considered [stable releases by Composer](https://getcomposer.org/doc/articles/versions.md#stability-constraints).

#### Abandoned Packages

Private Packagist will notify you as soon as a package in your Private Packagist organization gets marked as abandoned.
You can specify which private packages you want to get notifications for and whether you want to receive notifications for mirrored packages.

Private packages will get marked as abandoned as soon as the [abandoned property](https://getcomposer.org/doc/04-schema.md#abandoned) is set in the composer.json or for GitHub repositories as soon the repository has been archived.
Third party mirrored repositories can also set the abandoned property in the composer.json and Private Packagist will automatically mark packages as abandoned if they get removed from the mirrored third party repository.


#### Security Alerts

[Security Monitoring](./security-monitoring.md) allows you to receive notifications when security issues are found in dependencies of selected projects.

#### Security Summaries

In addition to immediate security alerts you can also receive either weekly or monthly summaries listing all open security issues in dependencies of your organization's monitored projects.

### Configuring Notifications
Every user receives security notifications by email for all projects they have access to by default.
Users can unsubscribe either from individual projects or from all security notifications if they do not wish to receive email notifications.

Notification channels allow you to receive notifications via other means than email to user accounts. The following types of notification channels are available:
- **Email**: Sends notifications to a list of email addresses
- **Slack Webhook**: Sends notifications to your configured Slack channel
- **Microsoft Teams Webhook**: Sends notifications to your configured Microsoft Teams channel
- **Webhook**: Sends an HTTP POST request to a defined URL optionally signed with a user supplied secret.

Notification channels can be added on your organization’s settings page under *Notification Channels -> Add Notification Channel*.

### Receiving Webhook Notifications

Webhook notifications are sent as HTTP POST requests to the endpoint configured with the notification event data send as payload.
HTTP, server, and network errors will automatically be retried up to five times.

#### Delivery Headers

HTTP POST payloads that are delivered to your webhook's configured URL endpoint will contain several special headers:

<table>
    <thead>
        <tr>
            <th>Header</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Packagist-Event</td>
            <td>Name of the notification event</td>
        </tr>
        <tr>
            <td>Packagist-Notification</td>
            <td>Notification identifier, allows you to track a notification across multiple retries</td>
        </tr>
        <tr>
            <td>Packagist-Delivery</td>
            <td>Identifier for the current request/delivery</td>
        </tr>
        <tr>
            <td>Packagist-Signature</td>
            <td>Signature to validate the request based on the payload. This header will only be sent if a webhook secret is configured.</td>
        </tr>
    </tbody>
</table>

#### Webhook Request Validation

We recommend that you set up a webhook secret and validate the payload either using [our api client](https://github.com/packagist/private-packagist-api-client#validate-incoming-webhook-payloads) or by running `hash_equals('sha1='.hash_hmac('sha1', (string) $request->getBody(), $SECRET_USER_CHOSEN), $response->getHeader('Packagist-Signature'));`

#### Webhook Example Payloads

Every webhook notification channel has a deliveries section which shows you the most recent notifications the channel received. You can also resend previous notifications.

##### Test Notification
Test notification to help you validate the setup of your webhook endpoint. You can send the notification by clicking on the "Trigger Test" button.

WEBHOOK_EXAMPLE[test]

##### Package Release Notification

Triggered every time Private Packagist finds one or more releases of a single package matching the criteria of the notification channel.

WEBHOOK_EXAMPLE[package-release]

##### Abandoned Package Notification

Triggered every time a package gets marked as abandoned.

WEBHOOK_EXAMPLE[package-abandoned]

##### Security Issue Notification

Triggered every time Private Packagist finds one or more security issues for a single project.

WEBHOOK_EXAMPLE[package-security-issue]

##### Security Single Issue Notification

Triggered every time Private Packagist finds a security issue for a single project. If configured, 
this will be sent instead of the regular security issues webhook which aggregates issues found at the same time. This is useful if your target cannot parse object collections, e.g. Jira.

WEBHOOK_EXAMPLE[package-security-issue-single]

##### Security Summary Notification

A weekly or monthly summary notification containing all open security issues for all projects in your organization.

WEBHOOK_EXAMPLE[package-security-issue-summary]
