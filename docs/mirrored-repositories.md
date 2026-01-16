# Mirrored Third-Party Repositories
##

Private Packagist can mirror packages from third-party Composer repositories like packagist.org, repo.magento.com, or any other Composer repository. Mirrored packages are stored in your Private Packagist organization, making your deployments independent of third-party repository uptime and ensuring faster, more reliable downloads.

By default, packagist.org is enabled as a mirrored repository for all organizations.

## Adding a Mirrored Repository

You can add additional mirrored repositories on your organization's settings page under *Mirroring*.

For private repositories that require authentication, first create credentials under *Settings > Manage Credentials*, then add the mirrored repository and select the credential during setup.

After adding the new mirrored repository, you can then remove the third party mirored repository from your "repositories" section of your composer.json.  
Run `composer update mirrors` in your projects to update the download locations in your lock file to point to your Private Packagist repository instead. 

See also: [Mirroring Magento Marketplace Packages](mirror-magento-marketplace) for a detailed walkthrough.

## Repository Priority

When you have multiple mirrored repositories, you can reorder them by dragging and dropping on the *Mirroring* settings page. The order determines which repository's packages are used when multiple repositories provide the same package.

Repositories at the top of the list have higher priority. When a package exists in multiple repositories, the repository higher in the list will be used to mirror that package. Once a package is mirrored from one repository, it cannot be added from another repository.

packagist.org always appears at the bottom of the list and has the lowest priority, ensuring that packages from your private repositories or other mirrors are preferred over the public versions.

Note that all repositories are still queried for their metadata and to check if they have the package being mirrored, regardless of their priority order. Priority only determines which repository's package gets used when multiple repositories have it.

## Permission Levels

When adding a mirrored repository, you can choose how packages from this repository should be added to your organization:

**Packages are automatically added when used** - Packages from this repository are automatically added when used with `composer update` or `composer require`. They are not added on `composer install`. This is the recommended setting for most repositories.

Note: Automatic mirroring requires authentication tokens with update access. Read-only tokens, commonly used in CI/CD environments, cannot trigger automatic mirroring.

**Packages must be manually managed** - Packages are not automatically mirrored. Any member of the organization can add packages from this repository through the Private Packagist interface or API.

**Packages must be managed by Admins or Owners** - Packages are not automatically mirrored. Only organization admins or owners can add packages from this repository. Useful for repositories that require approval before use.

**Mirror is disabled** - No packages can be added from this repository. The repository will not be queried for new packages. Existing packages from this repository remain accessible.

## Finding Packages in Mirrored Repositories

When mirroring a new package, Private Packagist queries **all enabled mirrored repositories in parallel** to find which ones have the package.

This happens because Private Packagist doesn't know in advance which repository contains a specific package. Querying all repositories at once is much faster than checking them one by one - it only takes as long as the slowest response instead of the sum of all response times. For example, if you have 20 repositories and the package is only in the last one, parallel querying is significantly faster than checking each repository sequentially.

This is why you may see credential errors for all configured repositories when running Composer commands, even if a package could be satisfied from a single repository. All repositories are being queried simultaneously to locate the package.

Once a package is mirrored from one repository, all versions come from that single source. Private Packagist does not combine versions from multiple repositories.

## Managing Repositories

### Disabling a Repository

If a mirrored repository becomes inaccessible or you no longer want to use it, you can disable it by changing its permission level to "Disabled".

**What happens when you disable a repository:**
- The repository is no longer queried when running `composer update` or `composer require`
- Existing packages from this repository remain in your organization and can still be installed
- Credential errors from this repository will no longer appear
- You can re-enable the repository later by changing its permission level

**When to disable:** Use this when a repository has credential issues but you want to keep the packages you've already mirrored.

### Deleting a Repository

You can delete a mirrored repository through the repository settings page.

**What happens when you delete a repository:**
- All packages that were mirrored from this repository are permanently deleted from your organization
- These packages will no longer be available to your developers
- Projects depending on these packages will fail to install until you restore the packages from another source

**When to delete:** Only delete a repository if you no longer need any of the packages from it. If you're unsure, disable it instead.

## Troubleshooting

##### Credential errors for all repositories

If you see credential errors for multiple repositories when running Composer commands, this is expected behavior. As explained above, Composer checks all repositories to build a complete version catalog. Fix the credentials for the failing repositories or disable them if they're no longer needed.

##### Packages not updating

Mirrored repositories do not provide webhook notifications. Packages from third-party repositories (other than packagist.org) are automatically updated every 12 hours. You can manually trigger an update for a package at any time through the package page or API.

##### Can't remove a repository with existing packages

To prevent accidental data loss, you must explicitly choose to delete a repository's packages during the deletion process. If you want to keep the packages, disable the repository instead.

## See Also

- [Mirroring Magento Marketplace Packages](mirror-magento-marketplace) - Detailed walkthrough for setting up Magento Marketplace mirroring
- [Composer Authentication](composer-authentication) - Authentication token types and their permissions
