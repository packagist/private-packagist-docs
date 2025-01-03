# GitHub, Bitbucket, GitLab and Other Integrations
## Synchronization of Users, Teams, Permissions and Repositories

Composer has broad support for version control systems, code hosting platforms and authentication protocols. As a consequence **Private Packagist is compatible with all the same systems and platforms**, namely Git, Mercurial or Subversion using SSH access, HTTP Basic Auth over SSL, or their native protocols.

To simplify the initial setup and maintenance of a Private Packagist account, we offer optional synchronization for GitHub organizations, Bitbucket workspaces and GitLab groups. This **integration keeps teams, their members, and access permissions in sync** with a matching GitHub organization. So you only need to manage users and permissions in a single place.

You can log into Private Packagist using an account on GitHub.com, Bitbucket.org, or GitLab.com. If you already have an account you can connect these services to your existing account on your [profile page](https://packagist.com/profile).

You can then either create a new organization directly synchronized with a GitHub organization, Bitbucket workspace or a GitLab group, or you can enable synchronization for an existing Organization in _Settings_.

![Synchronization](/Resources/public/img/docs/features/Sync-20241218.png)

Apart from synchronizing teams, users, and permissions, setting up the integration will simplify the addition of new packages to your Composer repository. When you create a **new repository** on GitHub, Bitbucket, or GitLab, it will be **added as a Composer package automatically** if it contains a _composer.json_ file. If you’d like to add existing repositories as packages, you can do so with the click of a button on the Packages tab in your organization.

**Update web hooks** notifying Packagist of new code pushes will be installed in the background, and **credentials to access the package** will be **configured automatically**.

## Integrations

Private Packagist integrates with the following systems:

#### GitHub
* OAuth: Users authenticate on Private Packagist with their GitHub accounts. If you use Private Packagist Self-Hosted, first create a GitHub app by following these [steps](../docs/self-hosted/github-integration-setup.md). 
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitHub organization
* Code Credentials: GitHub App or GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

#### GitHub Enterprise
* OAuth: Users authenticate on Private Packagist with their GitHub accounts. Follow [this setup](../docs/cloud/github-integration-setup.md) for Cloud plans or [this setup](../docs/self-hosted/github-integration-setup.md) for Self-Hosted.
* Synchronization:
    * Keeps team members and access permissions in sync with your GitHub Enterprise organizations
* Code Credentials: GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

#### Bitbucket Cloud (bitbucket.org)
* OAuth: Users authenticate on Private Packagist with their Bitbucket accounts. If you use Private Packagist Self-Hosted, first create a Bitbucket app by following these [steps](../docs/self-hosted/bitbucket-integration-setup.md).
* Synchronization:
    * Keeps groups, their members, and access permissions in sync with your Bitbucket workspace
* Code Credentials: Bitbucket API Key or Bitbucket App Password
* Webhooks: Code changes and releases

#### Bitbucket Data Center / Server

* OAuth: Users authenticate on Private Packagist with their Bitbucket Data Center / Server accounts. Follow [this setup](../docs/cloud/bitbucket-server-integration-setup.md) for Cloud plans or [this setup](../docs/self-hosted/bitbucket-server-integration-setup.md) for Self-Hosted.
* Synchronization:
    * Keeps users and access permissions in sync with your Bitbucket Server projects
    * Individual collaborators aren't supported
* Code Credentials: personal access token which are available since Bitbucket Server 5.5 or username and password for older versions
* Webhooks: Code changes and releases

#### GitLab
* OAuth: Users authenticate on Private Packagist with their GitLab accounts. If you use Private Packagist Self-Hosted, first create a GitLab app by following these [steps](../docs/self-hosted/gitlab-integration-setup.md).
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitLab groups
    * Individual collaborators aren't supported
* Code Credentials: GitLab API token
* Webhooks: Code changes and releases

#### GitLab (Self-hosted)
* OAuth: Users authenticate on Private Packagist with their GitLab accounts. Follow [this setup](../docs/cloud/gitlab-integration-setup.md) for Cloud plans or [this setup](../docs/self-hosted/gitlab-integration-setup.md) for Self-Hosted.
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitLab groups
    * Individual collaborators aren't yet supported
* Code Credentials: GitLab API Token
* Webhooks: Code changes and releases

#### AWS CodeCommit
* Code Credentials: Either create HTTPS Git credentials and store it as HTTP Basic credential or grant us access via SSH key
* Webhook:
    * Users need to [create an AWS CodeCommit Trigger](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-sns.html) using the generic hook URL from the package page
    * Code changes are available automatically. For releases, you need to select "All repository events" for the events type.

#### Azure DevOps
* Code Credentials: Either create a personal access token on Azure and store it as HTTP Basic credential or grant us access via SSH key
* Webhooks:
    * Users need to [create a Service Hook](https://docs.microsoft.com/en-us/azure/devops/service-hooks/services/webhooks?view=azure-devops) using the generic hook URL from the package page
    * Select the "Code pushed" event (includes commits and tags) and the repository that matches the package. No authentication or additional configuration is necessary.
