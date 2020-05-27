# GitHub, Bitbucket, GitLab and Other Integrations
## Synchronization of Users, Teams, Permissions and Repositories

Composer has broad support for version control systems, code hosting platforms and authentication protocols. As a consequence **Private Packagist is compatible with all the same systems and platforms**, namely Git, Mercurial or Subversion using SSH access, HTTP Basic Auth over SSL, or their native protocols.

To simplify the initial setup and maintenance of a Private Packagist account, we offer optional synchronization for GitHub organizations, Bitbucket teams and GitLab groups. This **integration keeps teams, their members, and access permissions in sync** with a matching GitHub organization. So you only need to manage users and permissions in a single place.

You can log into Private Packagist using an account on GitHub.com, Bitbucket.org, or GitLab.com. If you already have an account you can connect these services to your existing account on your [profile page](https://packagist.com/profile).

You can then either create a new organization directly synchronized with a GitHub organization, Bitbucket team or a GitLab group, or you can enable synchronization for an existing Organization in _Settings_.

![Synchronization](/Resources/public/img/docs/features/Sync-20170306.png)

Apart from synchronizing teams, users, and permissions, setting up the integration will simplify the addition of new packages to your Composer repository. When you create a **new repository** on GitHub, Bitbucket, or GitLab, it will be **added as a Composer package automatically** if it contains a _composer.json_ file. If you’d like to add existing repositories as packages, you can do so with the click of a button on the Packages tab in your organization.

**Update web hooks** notifying Packagist of new code pushes will be installed in the background, and **credentials to access the package** will be **configured automatically**.

## Integrations

Private Packagist integrates with the following systems:

#### GitHub
* OAuth: Users authenticate on Private Packagist with their GitHub accounts
* Synchronization:
    * Keeps team members and access permissions in sync with your GitHub organisations
    * Synced repositories use the default repository permission on GitHub
* Credentials: GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

#### GitHub Enterprise
* OAuth: Users authenticate on Private Packagist with their Bitbucket accounts. Please contact us to set this up for the cloud plan or follow [this setup](../docs/enterprise/github-integration-setup.md) for enterprise.
* Synchronization:
    * Keeps team members and access permissions in sync with your GitHub Enterprise organisations
* Credentials: GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

#### Bitbucket.org / Cloud
* OAuth: Users authenticate on Private Packagist with their Bitbucket.org accounts
* Synchronization:
    * Keeps users permissions in sync with your Bitbucket groups
    * Group and user changes aren't kept in sync
* Credentials: Bitbucket API Key or Bitbucket App Password
* Webhooks: Code changes

#### Bitbucket / Stash Server
* OAuth: Users authenticate on Private Packagist with their Bitbucket accounts. Please contact us to set this up for the cloud plan or follow [this setup](../docs/enterprise/bitbucket-server-integration-setup.md) for enterprise.
* Synchronization:
    * Keeps users and access permissions in sync with your Bitbucket projects
    * Individual collaborators aren't supported
    * New projects or team changes aren't kept in sync
* Credentials: SSH keys
* Webhooks: Code changes

#### GitLab
* OAuth: Users authenticate on Private Packagist with their GitLab accounts
* Synchronization:
    * Keeps access permissions in sync with your Gitlab groups
    * Individual collaborators aren't kept in sync
    * New projects or team changes aren't kept in sync
* Credentials: GitLab API token
* Webhooks: Code changes

#### GitLab (Self-hosted)
* OAuth: Users authenticate on Private Packagist with their Bitbucket accounts. Please contact us to set this up for the cloud plan or follow [this setup](../docs/enterprise/gitlab-integration-setup.md) for enterprise.
* Synchronization:
    * Keeps access permissions in sync with your Gitlab groups
    * Individual collaborators aren't kept in sync
    * New projects or team changes aren't kept in sync
* Credentials: GitLab API Token
* Webhooks: Code changes

#### AWS CodeCommit 
* Webhook: Supports [AWS SNS subscription confirmation](https://docs.aws.amazon.com/sns/latest/dg/sns-message-and-json-formats.html)