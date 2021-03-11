# Synchronizations FAQ
##

In Private Packagist synchronizations is how you keep packages, teams, their members, and access permissions in sync with your version control system like GitHub, GitLab or Bitbucket.

You can find below answers for frequently-asked questions concerning synchronizations.

## How to migrate my synchronization from GitHub to Bitbucket?

1. Add the new synchronization from Bitbucket.
2. Delete the packages that were created from the GitHub synchronization. You can filter by the GitHub synchronization on the package list page and then you can delete the packages from that list.
3. Make sure the new synchronization from Bitbucket successfully run. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. Then you can safely delete the old synchronization from GitHub.

You can follow the same steps above in case you want to migrate from Bitbucket to GitLab or the other way around.

## How to migrate from one organization to another one in GitHub?

1. Add the new synchronization from the new GitHub organization.
2. On GitHub, you can transfer the packages from the old organization to the new one.
3. In case the old synchronization was marked as primary, you need to promote the new synchronization as primary otherwise you won't be able to delete it. After verifying that the new synchronization is in sync with Private Packagist, you can remove the old GitHub organization synchronization.

## How to migrate from one workspace to another one in Bitbucket?

1. Transfer your repositories in Bitbucket from the old workspace to the new one.
2. Create a new synchronization with the new bitbucket workspace.
3. After verifying that the new synchronization has the correct data, you can remove the old workspace synchronization.

## How to migrate from one group to another one in Gitlab?
1. Create a new synchronization with the new Gitlab workspace.
2. Transfer your repositories in Gitlab from the old group to the new one.
3. Run the new GitLab group synchronization.
4. After verifying that the new synchronization has the correct data, you can remove the old workspace synchronization.
