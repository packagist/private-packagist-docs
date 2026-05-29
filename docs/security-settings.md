# Security settings

Your organization's security settings page (_Settings > Security_) controls security-related options for how Private Packagist serves packages to Composer. 

In organizations with suborganizations, these security setting can only be changed on the top-level organization. The chosen values are then propagated to all suborganizations so that the entire organization tree shares the same fallback behavior.

This page documents the available settings and the trade-offs to consider when changing them.

### Legacy insecure package download fallback

Private Packagist replaces all dist/artifact download URLs for packages with its own URLs so that every download goes through Private Packagist and benefits from its access controls, mirroring, and integrity guarantees.

The _Enable legacy insecure package download fallback to original dist file URLs_ setting lets you re-add the original upstream URL alongside the Private Packagist URL. When Private Packagist is not accessible or returns errors, Composer can then fall back to downloading the artifact from its original location — for example a third-party mirrored repository download URL, or a GitHub API endpoint that generates a zip file from the repository.

This used to be the default behavior, but it is insecure: a fallback download circumvents the protections Private Packagist applies to dist/artifact files, including access controls and any future integrity checks. For that reason it is disabled by default for new organizations and we recommend leaving it disabled unless you have a specific reliability requirement that justifies the trade-off.

### Hide source code checkout URLs from Composer

Each version in a Composer repository can advertise a source code checkout URL alongside the dist file URLs. The source references a commit in the upstream VCS repository and is persisted in your `composer.lock` file. Composer uses this URL as a fallback when the dist file URL is inaccessible, bypassing Private Packagist's access controls. 

That's why Private Packagist hides the source for all mirrored packages by default, while the source URL is still available for your own and private packages. 

You can control this behavior using the `Hide source code checkout URLs` setting using one of these options:

- **Never**: Disables hiding source code checkout URLs entirely. If the source checkout URL of the package is known, it will be included in package metadata.
- **Only for packages mirrored from Packagist.org**
- **For all mirrored packages**
- **Always**: Hides all source code checkout URLs, including your own and private packages. 

After changing this setting, run `composer update mirrors` in your projects to update the package metadata in your lock file, so the source code checkout URLs are removed (or added back) according to the selected option.
