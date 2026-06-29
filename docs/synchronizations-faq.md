# Synchronizations
## Synchronization of users, teams, permissions and repositories

To simplify the initial setup and maintenance of a Private Packagist organization, we offer optional synchronization for GitHub organizations, 
Bitbucket workspaces and GitLab groups. This synchronization keeps teams, their members, and access permissions in sync. 
So you only need to manage users and permissions in a single place.

Apart from synchronizing teams, users, and permissions, setting up the synchronization will simplify the addition of new packages 
to your Composer repository. When you create a **new repository** on GitHub, Bitbucket, or GitLab, it will be **added as a 
Composer package automatically** if it contains a _composer.json_ file. If you’d like to add existing repositories as packages, 
you can do so with the click of a button on the Packages tab in your organization.

**Update web hooks** notifying Packagist of new code pushes will be installed in the background, and **credentials to access
the package** will be **configured automatically**.

## An integration is required

Every synchronization is built on top of an OAuth integration with a code hosting platform, and you cannot set up a synchronization without one. The integration is what:

* **Defines the platform to synchronize with.** The integration stores the base URL of the code hosting platform, for example the address of your self-managed GitLab or Bitbucket Data Center instance. Without it, Private Packagist would not know which server to talk to.
* **Makes it possible to list what you can synchronize.** Creating a synchronization requires you to be authenticated with the platform through OAuth, because that is the only way for Private Packagist to list the organizations, workspaces, or groups available to you to synchronize.
* **Controls who can access the synchronized data.** A synchronization only grants access to the users who have access on the code hosting platform and who have connected their account to the same integration. Without the integration, the synchronized teams, permissions, and packages would not be accessible to anyone. See ["Accessing packages from Synchronizations across multiple integrations"](#accessing-packages-from-synchronizations-across-multiple-integrations).

Each code hosting platform needs its own integration. For example if you synchronize with GitHub and later also want to synchronize with GitLab, you first need to set up a separate GitLab integration.

On Private Packagist Cloud, the integrations for the default platforms (github.com, bitbucket.org, and gitlab.com) are preconfigured. On Private Packagist Self-Hosted none are preconfigured, so you must create the integration yourself before you can synchronize.

On Private Packagist Self-Hosted, deleting an integration in the admin panel also deletes every synchronization that depends on it. In an organization's own settings, an integration that is still used by a synchronization cannot be deleted until you remove those synchronizations first.

## Getting started

You can log into Private Packagist using an account on GitHub.com, Bitbucket.org, or GitLab.com with the relevant [OAuth integration](/features/integration-github-bitbucket-gitlab)
If you already have an account you can connect these services to your existing account on your [profile page](https://packagist.com/profile).

You can then either create a new organization directly synchronized with a GitHub organization, Bitbucket workspace or a GitLab group,
or you can enable synchronization for an existing Organization in _Settings_.

![Synchronization](/Resources/public/img/docs/features/Sync-20241218.png)

## Accessing packages from Synchronizations across multiple integrations

Organization members can only access packages from synchronizations with integrations connected to their account.

To make sure members can access the packages from all synchronizations from multiple integrations:

1. All members should go to their User Profile
2. Under "Connected Accounts" connect to each integration that powers the synchronizations they need

## Supported providers 

### GitHub
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitHub organization
* Code Credentials: GitHub App or GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

### GitHub Enterprise Server
* Synchronization:
    * Keeps team members and access permissions in sync with your GitHub Enterprise organizations
* Code Credentials: GitHub API Token
* Webhooks: Code changes, releases, created repositories, team creation or member changes

### Bitbucket Cloud (bitbucket.org)
* Synchronization:
    * Keeps groups, their members, and access permissions in sync with your Bitbucket workspace
* Code Credentials: Bitbucket API Token
* Webhooks: Code changes and releases

### Bitbucket Data Center / Server
* Synchronization:
    * Keeps users and access permissions in sync with your Bitbucket Server projects
    * Individual collaborators aren't supported
* Code Credentials: personal access token which are available since Bitbucket Server 5.5 or username and password for older versions
* Webhooks: Code changes and releases

#### GitLab
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitLab groups
    * Individual collaborators aren't supported
* Code Credentials: GitLab API token
* Webhooks: Code changes and releases

#### GitLab Self-Managed
* Synchronization:
    * Keeps teams, their members, and access permissions in sync with your GitLab groups
    * Individual collaborators aren't yet supported
* Code Credentials: GitLab API Token
* Webhooks: Code changes and releases

## Frequently asked questions

### What happens if you promote a synchronization to a primary synchronization?

Synchronized Private Packagist organizations automatically have all admins and owners of the remote organization of the primary synchronization assigned to the admins and owners teams in Private Packagist. Admins and owners of additional synchronized remote organizations are not added to the admins or owners teams in Private Packagist.

When promoting a synchronization to primary, admins and owners of the corresponding remote organization will now be the sole members of the admins and owners teams on Private Packagist.
So keep in mind that if you switch the primary synchronization to a different service (e.g. from GitHub to Bitbucket), all admins and owners who do not have their user accounts connected to the new service yet (Bitbucket in the example) will lose admin/owner access to the Private Packagist organization. To restore their access, the users have to connect their account to the new service on the profile page.

### How to migrate a synchronization to another code hosting platform?

You can follow these same steps in case you want to migrate between two different code hosting platforms. For example, from Bitbucket to GitLab or the other way around, from GitHub to Bitbucket or from one self-hosted GitLab instance to another.

In the steps below, we'll consider a migration from GitHub to Bitbucket as an example: 

1. Add a new synchronization with your Bitbucket workspace.
2. Ensure that all members keep the access to the packages by following:  ["Accessing packages from Synchronizations across multiple integrations"](#accessing-packages-from-synchronizations-across-multiple-integrations)
3. Delete the packages that were created by the GitHub synchronization. You can filter by the GitHub synchronization on the package list page and then you can delete the packages from that list.
4. Make sure the new synchronization with your Bitbucket workspace successfully ran. In case the old synchronization was primary, you need to promote the new synchronization to primary. Otherwise you won't be able to delete your old synchronization. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.
5. Delete the old synchronization from GitHub.

### How to migrate from one organization to another one in GitHub?

1. Add a new synchronization with the new GitHub organization.
2. On GitHub, you can transfer the packages from the old organization to the new one.
3. In case the old synchronization was primary, you need to promote the new synchronization to primary otherwise you won't be able to delete the old one. After verifying that the new synchronization successfully ran you can remove the old synchronization with GitHub. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

### How to migrate from one workspace to another one in Bitbucket?

1. Transfer your repositories in Bitbucket from the old workspace to the new one.
2. Create a new synchronization with the new Bitbucket workspace.
3. In case the old synchronization was primary, you need to promote the new synchronization to primary. Otherwise you won't be able to delete the old synchronization. After verifying that the new synchronization successfully ran, you can remove the old synchronization with Bitbucket. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

### How to migrate from one group to another one in GitLab?

1. Create a new synchronization with the new GitLab workspace.
2. Transfer your repositories in GitLab from the old group to the new one.
3. Run the new GitLab group synchronization.
4. In case the old synchronization was primary, you need to promote the new synchronization to primary otherwise you won't be able to delete the old synchronization. After verifying that the new synchronization successfully ran, you can remove the old synchronization with GitLab. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

### How to switch the credentials used by a synchronization?

You don't need to recreate the synchronization to change its credentials.

To rotate a token, edit the credential directly: under _Settings > Credentials_, open the credential and enter the new token.

To change the synchronization to use a different credential:

1. Under _Settings > Credentials_, add the new credential.
2. Still under _Settings > Credentials_, click delete on the old credential. A dialog opens where you can select the new credential to use instead.

In both cases the affected synchronizations run again automatically.

### How to switch a Bitbucket App Password (deprecated) to a Bitbucket API Token?

Bitbucket is phasing out App Passwords in favor of API Tokens. You can switch in place without recreating your synchronization:

1. Create an API Token in Bitbucket with the same scopes as your App Password.
2. Under _Settings > Credentials_, open your existing _Bitbucket App Password_ credential, enter your _Bitbucket email address_ and paste the new _API token_. Private Packagist recognizes the API token and switches the credential type to _Bitbucket API Token_ for you automatically.

The affected synchronizations run again automatically with the new token.
