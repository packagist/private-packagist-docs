# Notifications

### Notification Types

#### Package Releases

Private Packagist will notify you every time we find one or more new releases for one of your packages.
Notification channels allow you to specify about which private packages you want to get notified and whether you also want to receive notifications for mirrored packages as well.
Additionally, you can filter for releases by minimum stability: "dev / any" will match any release including commits to a branch whereas "stable" will only match releases considered stable releases by Composer.

#### Security Alerts

[Security Monitoring](./security-monitoring.md) allows you to select notification channels to receive a notification every time security issues are found in dependencies of one of your projects.

#### Security Summaries

In addition to immediate security alerts your notification channels can also receive either weekly or monthly summaries listing all open security issues in dependencies of your organization's projects.

### Configuring Notifications
Every user receives security notifications by email for all projects they have access to by default.
Users can unsubscribe either from individual projects or from all security notifications if they do not wish to receive email notifications.

Notification channels allow you to receive security notifications via other means than email to user accounts. The following types of notification channels are available:
- **Email**: Sends notifications to a list of email addresses
- **Slack Webhook**: Sends notifications to your configured Slack channel
- **Microsoft Teams Webhook**: Sends notifications to your configured Microsoft Teams channel
- **Webhook**: Sends an HTTP POST request to a defined URL optionally signed with a user supplied secret.

Notification channels can be added on your organization’s settings page under *Notification Channels -> Add Notification Channel*.

### Receiving Webhook Notifications

Webhook notifications are sent as HTTP POST requests to the endpoint configured with the notification event data send as payload.
Client, server, and network errors will automatically be retried up to five times.

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

WEBHOOK_EXAMPLE[package:release]

##### Security Issue Notification

Triggered every time Private Packagist finds one or more security issues for a single project.

WEBHOOK_EXAMPLE[package:security-issue]

##### Security Summary Notification

A weekly or monthly summary notification containing all open security issues for all projects in your organization.

WEBHOOK_EXAMPLE[package:security-issue:summary]
