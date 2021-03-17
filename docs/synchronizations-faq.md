# Synchronizations FAQ
##

In Private Packagist synchronizations is how you keep packages, teams, their members, and access permissions in sync with your version control system like GitHub, GitLab or Bitbucket.

You can find below answers for frequently-asked questions concerning synchronizations.

## What happens if you promote a synchronization to a primary synchronization?

Only with a primary synchronization that admins and owners teams will be added to Private Packagist.

The following are some things to consider when promoting a synchronization to a primary synchronization:
- Owners and Admins teams in Private Packagist will be the same ones from the primary synchronization.
- In case you change the integration (e.g. From GitHub to Bitbucket), you'll have to go to your profile page and connect your Private Packagist account with the new integration. Otherwise, you'll lose access to your account.
- You will not be able to delete the primary synchronization.

## How to migrate a synchronization from GitHub to Bitbucket?

1. Add the new synchronization from Bitbucket.
2. Delete the packages that were created from the GitHub synchronization. You can filter by the GitHub synchronization on the package list page and then you can delete the packages from that list.
3. Make sure the new synchronization from Bitbucket successfully run. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. Please refer to [this section](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.
4. Safely delete the old synchronization from GitHub.

You can follow the same steps above in case you want to migrate from Bitbucket to GitLab or the other way around.

## How to migrate from one organization to another one in GitHub?

1. Add the new synchronization from the new GitHub organization.
2. On GitHub, you can transfer the packages from the old organization to the new one.
3. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. After verifying that the new synchronization is in sync with Private Packagist, you can remove the old GitHub organization synchronization. Please refer to [this section](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

## How to migrate from one workspace to another one in Bitbucket?

1. Transfer your repositories in Bitbucket from the old workspace to the new one.
2. Create a new synchronization with the new bitbucket workspace.
3. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. After verifying that the new synchronization is in sync with Private Packagist, you can remove the old Bitbucket workspace synchronization. Please refer to [this section](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

## How to migrate from one group to another one in Gitlab?

1. Create a new synchronization with the new Gitlab workspace.
2. Transfer your repositories in Gitlab from the old group to the new one.
3. Run the new GitLab group synchronization.
4. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. After verifying that the new synchronization is in sync with Private Packagist, you can remove the old Gitlab group synchronization. Please refer to [this section](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.
