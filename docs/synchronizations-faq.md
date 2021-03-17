# Synchronizations FAQ
##

In Private Packagist synchronizations is how you keep packages, teams, their members, and access permissions in sync with your version control system like GitHub, GitLab or Bitbucket.

You can find below answers for frequently-asked questions concerning synchronizations.

## What happens if you promote a synchronization to a primary synchronization?

Private Packagist organizations that are synchronized with a remote organization have automatically all admins and owners of the synchronization assigned to the admins and owners team in Private Packagist.

If an organization is synchronized with multiple remote organizations then only admins and owners of the primary synchronization are added to the respective teams.

When promoting a synchronization to a primary synchronization, new admins and owners will be added from the primary synchronization to Private Packagist.
Users who are not in the new team will lose access to the organization. To restore their access, the user have to connect their account to the new service from the profile page.

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
