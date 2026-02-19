# Package Lifecycle

This page explains how Private Packagist handles package versions, updates, and deletions for both private packages and mirrored packages.

## Private Packages

### How versions are created

For packages added from VCS repositories, Private Packagist reads version information from tags and branches:

- **Tags** become versions (e.g., tag `v1.2.3` becomes version `v1.2.3`, but you can refer to it as `1.2.3`)
- **Branches** become dev versions (e.g., branch `main` becomes `dev-main`)

See [Troubleshooting](troubleshooting) if versions are not appearing as expected.

### Deleting a package

When you delete a private package from Private Packagist:

- The package can no longer be installed via Composer
- The package no longer appears in search results
- Dist files are deleted, so the package can no longer be downloaded from Private Packagist

If the package was a synchronized package (originally added through a [synchronization](synchronizations-faq)) and the repository still exists, it will **not** be re-added automatically during the next synchronization. To add it again, go to _Packages > Add Package_ and select your synchronization. Otherwise, go to _Packages > Add Package > By URL_ to add it back manually.

### When the VCS repository of a package is deleted

When the VCS repository is deleted on your code hosting platform:

- **Synchronized packages:** Private Packagist deletes the package once it receives a webhook notification or during the next synchronization. The same effects apply as manual deletion.
- **Packages added by URL:** The package is not automatically removed. It remains in your organization but updates will fail.

### When a tag is deleted

When a tag is removed from the repository:

- The corresponding version no longer appears on the package page
- The version is removed from repository metadata (`composer show --available` will not list it)
- Existing lock files can still install the version because dist files are retained

### When a tag is force-pushed

When a tag is force-pushed to point to a different commit:

- Private Packagist updates the metadata to reference the new commit
- Both the old and new dist files are preserved
- Existing lock files referencing the old commit continue to download the original code
- New or updated lock files reference the new commit and download the updated code

This behavior preserves deployment stability while allowing version updates when needed.

### Renaming a package

When you change the package's `name` property in composer.json, from `acme/package` to `acme-inc/other-package` for example: 

- Private Packagist marks `acme/package` as abandoned
- Private Packagist creates a new package with the new name `acme-inc/other-package`
- The old package `acme/package` suggests the new package `acme-inc/other-package` as a replacement
- Existing lock files referencing the old `acme/package` versions continue to download the correct code

## Mirrored Packages

This section describes how Private Packagist handles packages or versions that are deleted on packagist.org or other mirrored third-party repositories.

### When a package is removed

When a package is removed from packagist.org or another mirrored third-party repository, Private Packagist does **not** delete the package. Instead:

- The package is marked as abandoned with the reason "not found in mirror"
- An abandoned notification is sent to configured notification channels
- The package remains installable from existing lock files because Private Packagist retains all previously downloaded dist files

This protects your deployments from third-party deletions - one of the core benefits of mirroring.

### When a version is removed

When a specific version is removed from packagist.org or another mirrored third-party repository:

- The version is removed from the package metadata and no longer appears in `composer show --available`
- The version is not shown on the package page on packagist.com
- The version is no longer available during `composer update`
- Existing lock files can still install the version because dist files are retained

### When a version is republished with different content

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
