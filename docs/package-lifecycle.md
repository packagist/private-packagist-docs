# Package Lifecycle

This page explains how Private Packagist handles package versions, updates, and deletions for both private packages and mirrored packages.

## Private Packages

### How Versions Are Created

For packages added from VCS repositories, Private Packagist reads version information from tags and branches:

- **Tags** become versions (e.g., tag `v1.2.3` becomes version `1.2.3`)
- **Branches** become dev versions (e.g., branch `main` becomes `dev-main`)

See [Troubleshooting](troubleshooting) if versions are not appearing as expected.

### Deleting a Package

When you delete a private package from Private Packagist:

- The package can no longer be installed via Composer
- The package no longer appears in search results
- Dist files are retained, so existing lock files continue to work

If the package was originally added through a [synchronization](synchronizations-faq) and the repository still exists, it will **not** be re-added automatically. To add it again, go to _Packages > Add Package > Organization Packages_. Otherwise, go to _Packages > Add Package > By URL_ to add it back manually.

### When the Source Repository is Deleted

When the source repository is deleted on your code hosting platform:

- Private Packagist deletes the package once it receives a webhook notification or during the next synchronization
- The same effects apply as manual deletion: the package cannot be installed and no longer appears in search

### When a Tag is Deleted

When a tag is removed from the repository:

- The corresponding version no longer appears on the package page
- The version is removed from repository metadata (`composer show --available` will not list it)
- Existing lock files can still install the version because dist files are retained

### When a Tag is Force-Pushed

When a tag is force-pushed to point to a different commit:

- Private Packagist updates the metadata to reference the new commit
- Both the old and new dist files are preserved
- Existing lock files referencing the old commit continue to install the original code
- New installations of that version receive the updated code

This behavior preserves deployment stability while allowing version updates when needed.

## Mirrored Packages

This section describes how Private Packagist handles packages or versions that are deleted on packagist.org or other mirrored third-party repositories.

### When a Package is Removed

When a package is removed from packagist.org or another mirrored third-party repository, Private Packagist does **not** delete the package. Instead:

- The package is marked as abandoned with the reason "not found in mirror"
- An abandoned notification is sent to configured notification channels
- The package remains installable from existing lock files because Private Packagist retains all previously downloaded dist files

This protects your deployments from third-party deletions - one of the core benefits of mirroring.

### When a Version is Removed

When a specific version is removed from packagist.org or another mirrored third-party repository:

- The version is removed from the package metadata and no longer appears in `composer show --available`
- Existing lock files can still install the version because dist files are retained
- New installations cannot select this version

### When a Version is Republished with Different Content

Different mirrored repositories handle version integrity differently:

**packagist.org and other VCS-backed repositories:**

packagist.org serves metadata while dist files are hosted by GitHub, GitLab, or other code hosting platforms. The git commit hash serves as the integrity check. If a tag is force-pushed, the commit hash changes and Private Packagist updates the package metadata to reference the new commit.

**repo.magento.com and my.yoast.com:**

These repositories are known to occasionally change dist files after initial publication. Private Packagist handles this by not sending checksums to Composer for packages from these repositories. This prevents checksum validation errors while still allowing you to install packages.

**Other third-party repositories:**

For repositories that provide checksums, Private Packagist stores the checksum from the initial mirror. If the repository later changes the file content, Private Packagist continues serving the originally mirrored file with its original checksum. This ensures consistent installations and prevents validation errors, as the served file always matches the stored checksum.

## See Also

- [Composer Project Setup](setup) - How to add packages to your organization
- [Synchronizations](synchronizations-faq) - Automatic repository imports from GitHub, GitLab, and Bitbucket
- [Mirrored Third-Party Repositories](mirrored-repositories) - Adding and managing mirrored repositories
