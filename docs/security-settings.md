# Security settings

Your organization's security settings page (_Settings > Security_) controls how Private Packagist serves packages to Composer. The options let you close the download fallback paths Composer would otherwise use when a package cannot be downloaded from Private Packagist.

In organizations with suborganizations, these settings can only be changed on the top-level organization. The chosen values are propagated to all suborganizations so the whole organization tree shares the same fallback behavior.

## Why Composer's download fallbacks are a risk

### Composer download fallback behavior

Composer is built for resilience: if the preferred download location is unavailable, it tries the next one. For a project mirrored through Private Packagist, a single package in `composer.lock` can point at up to three download paths:

1. The preferred mirror URL on Private Packagist.
2. The original upstream dist URL, for example a zip from the GitHub API.
3. The git source repository, cloned as a last resort.

This rarely helps in practice. CI and deployment environments usually hold credentials for Private Packagist only, so when it is unreachable the fallbacks fail on authentication anyway. They succeed silently only for public third-party packages, which is where supply chain risk lives.

### Supply chain attack example

Suppose a malicious version of `psr/log` is published. Private Packagist detects it and returns a 404 for that version. Without the settings below, Composer treats the 404 as a transient failure, picks the next URL, and downloads the malicious artifact directly from GitHub. If that also fails, it falls back to a source checkout. Every protection Private Packagist applied is bypassed.

On the client side this is governed by `preferred-install` (prefers `dist` since Composer 2.1) and `source-fallback` (deprecated, defaults to `false` since Composer 2.10). Because these are client settings, you cannot rely on every environment configuring them securely. The settings below remove the fallback paths from the package metadata itself, so the protection applies regardless of the Composer version or client configuration.

## Legacy insecure package download fallback

Private Packagist replaces all dist/artifact download URLs with its own, so every download goes through Private Packagist and benefits from its access controls, mirroring, and integrity guarantees.

The _Enable legacy insecure package download fallback to original dist file URLs_ setting re-adds the original upstream URL as a dist mirror alongside the Private Packagist URL. When Private Packagist is unavailable or returns errors, Composer can then download the artifact from its original location, bypassing the protections Private Packagist applies.

It is disabled by default for new organizations, and we recommend leaving it disabled unless you have a specific reliability requirement that justifies the trade-off. Organizations created before this setting was introduced on June 1st, 2026 have it enabled by default to preserve their previous behavior. We recommend they disable it too.

After changing this setting, run `composer update mirrors` in your projects to rewrite their lock files so the dist URLs match the selected option.

### How your lock file changes

With the fallback enabled, the upstream URL is advertised as a dist mirror:

```json
"dist": {
    "type": "zip",
    "url": "https://api.github.com/repos/php-fig/log/zipball/abc1234",
    "mirrors": [
        {
            "url": "https://repo.packagist.com/my-org/dists/psr/log/abc1234.zip",
            "preferred": true
        }
    ],
    "reference": "abc1234"
}
```

With it disabled, the `mirrors` array is removed and Private Packagist is the sole dist URL:

```json
"dist": {
    "type": "zip",
    "url": "https://repo.packagist.com/my-org/dists/psr/log/abc1234.zip",
    "reference": "abc1234"
}
```

## Hide source code checkout URLs from Composer

Each version in a Composer repository can advertise a source code checkout URL alongside its dist URLs. The source references a commit in the upstream VCS repository and is persisted in `composer.lock`. Composer uses it as a fallback when the dist URL is inaccessible, bypassing Private Packagist's access controls.

Removing the `source` block prevents this fallback regardless of the Composer version or the client's `preferred-install` and `source-fallback` settings.

### Options

The _Hide source code checkout URLs_ setting offers:

- **Never**: source checkout URLs are always included when known.
- **Only for packages mirrored from Packagist.org**: hides the source for packagist.org packages only.
- **For all mirrored packages**: hides the source for every third-party mirrored package, while keeping it for your own and private packages.
- **Always**: hides all source code checkout URLs, including your own and private packages.

New organizations default to _For all mirrored packages_, which we recommend. Organizations created before this setting was introduced on June 1st, 2026 default to _Only for packages mirrored from Packagist.org_, and we recommend they switch to _For all mirrored packages_. Keeping the source for your own and private packages lets developers install them from source for local development and contributions. If your developers also contribute to third-party packages from source, _Only for packages mirrored from Packagist.org_ is a useful middle ground.

After changing this setting, run `composer update mirrors` in your projects to rewrite their lock files so the source URLs match the selected option.

### How your lock file changes

When the source is hidden, the `source` block is dropped and a third-party package keeps only its dist entry:

```json
"dist": {
    "type": "zip",
    "url": "https://repo.packagist.com/my-org/dists/psr/log/abc1234.zip",
    "reference": "abc1234"
}
```
