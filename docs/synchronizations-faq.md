# Synchronization FAQ
##

Synchronizations allow you to keep packages, teams, their members, and access permissions in sync with your code hosting platform, e.g. GitHub, GitLab or Bitbucket.

## What happens if you promote a synchronization to a primary synchronization?

Synchronized Private Packagist organizations automatically have all admins and owners of the remote organization of the primary synchronization assigned to the admins and owners teams in Private Packagist. Admins and owners of additional synchronized remote organizations are not added to the admins or owners teams in Private Packagist.

When promoting a synchronization to primary, admins and owners of the corresponding remote organization will now be the sole members of the admins and owners teams on Private Packagist.
So keep in mind that if you switch the primary synchronization to a different service (e.g. from GitHub to Bitbucket), all admins and owners who do not have their user accounts connected to the new service yet (Bitbucket in the example) will lose admin/owner access to the Private Packagist organization. To restore their access, the users have to connect their account to the new service on the profile page.

## How to migrate a synchronization from GitHub to Bitbucket?

1. Add a new synchronization with your Bitbucket workspace.
2. Delete the packages that were created by the GitHub synchronization. You can filter by the GitHub synchronization on the package list page and then you can delete the packages from that list.
3. Make sure the new synchronization with your Bitbucket workspace successfully ran. In case the old synchronization was primary, you need to promote the new synchronization to primary. Otherwise you won't be able to delete your old synchronization. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.
4. Delete the old synchronization from GitHub.

You can follow the same steps above in case you want to migrate from Bitbucket to GitLab or the other way around.

## How to migrate from one organization to another one in GitHub?

1. Add a new synchronization with the new GitHub organization.
2. On GitHub, you can transfer the packages from the old organization to the new one.
3. In case the old synchronization was primary, you need to promote the new synchronization to primary otherwise you won't be able to delete the old one. After verifying that the new synchronization successfully ran you can remove the old synchronization with GitHub. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

## How to migrate from one workspace to another one in Bitbucket?

1. Transfer your repositories in Bitbucket from the old workspace to the new one.
2. Create a new synchronization with the new Bitbucket workspace.
3. In case the old synchronization was primary, you need to promote the new synchronization to primary. Otherwise you won't be able to delete the old synchronization. After verifying that the new synchronization successfully ran, you can remove the old synchronization with Bitbucket. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.

## How to migrate from one group to another one in GitLab?

1. Create a new synchronization with the new GitLab workspace.
2. Transfer your repositories in GitLab from the old group to the new one.
3. Run the new GitLab group synchronization.
4. In case the old synchronization was primary, you need to promote the new synchronization to primary otherwise you won't be able to delete the old synchronization. After verifying that the new synchronization successfully ran, you can remove the old synchronization with GitLab. Please refer to ["What happens if you promote a synchronization to a primary synchronization?"](#what-happens-if-you-promote-a-synchronization-to-a-primary-synchronization) for more details about promoting a synchronization to primary.
