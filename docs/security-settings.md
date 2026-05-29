# Security settings

Your organization's security settings page (_Settings > Security_) controls security-related options for how Private Packagist serves packages to Composer. This page documents the available settings and the trade-offs to consider when changing them.

### Legacy insecure package download fallback

Private Packagist replaces all dist/artifact download URLs for packages with its own URLs so that every download goes through Private Packagist and benefits from its access controls, mirroring, and integrity guarantees.

The _Enable legacy insecure package download fallback to original dist file URLs_ setting lets you re-add the original upstream URL alongside the Private Packagist URL. When Private Packagist is not accessible or returns errors, Composer can then fall back to downloading the artifact from its original location — for example a third-party mirrored repository download URL, or a GitHub API endpoint that generates a zip file from the repository.

This used to be the default behavior, but it is insecure: a fallback download circumvents the protections Private Packagist applies to dist/artifact files, including access controls and any future integrity checks. For that reason it is disabled by default for new organizations and we recommend leaving it disabled unless you have a specific reliability requirement that justifies the trade-off.

In organizations with suborganizations, the setting can only be changed on the top-level organization. The chosen value is then propagated to all suborganizations so that the entire organization tree shares the same fallback behavior.
