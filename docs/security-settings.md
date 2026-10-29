# Security settings
## 

Your organization's security settings page (_Settings > Security_) controls how Private Packagist serves packages to Composer. The options let you close the download fallback paths Composer would otherwise use when a package cannot be downloaded from Private Packagist, restrict which Composer clients may access your Composer repository, limit which packages may act as Composer plugins, and hold back newly published package versions with a cooldown dependency policy.

In organizations with suborganizations, these settings can only be changed on the top-level organization. The chosen values are propagated to all suborganizations so the whole organization tree shares the same fallback behavior.

### When Composer's malware policy isn't enough

#### How Composer handles flagged malware

Composer 2.10 introduced [dependency policies](https://getcomposer.org/doc/06-config.md#policy) that handle malware on the client side. With the default policy, it removes flagged versions from the pool used by `composer update`, `composer require` and `composer create-project`, blocks them during `composer install` even when they're already present in a lock file, and reports them through `composer audit`. Combined with the Packagist.org malware feed, powered by [Aikido](https://www.aikido.dev/), this gives every Composer 2.10 user installing from Packagist.org rapid malware blocking by default.

That covers a developer running a current Composer with the default configuration, but two limitations remain:

- A project can disable the policy in its own `composer.json` with `"config": {"policy": {"malware": {"block": false}}}`.
- Composer versions older than 2.10 have no concept of dependency policies and continue installing flagged versions normally.

Because these are client settings, you cannot rely on every environment configuring them securely. Different environments could be running outdated Composer versions. The repository-wide settings below block Composer from installing flagged versions, so the protection applies regardless of the Composer version or client configuration.

### Malware blocking

Private Packagist refuses to serve the dist/artifact file for a malware-flagged version regardless of which Composer version requests it. When Composer requests the dist for a flagged version through `composer install` against an existing lock file or through `composer update` or `composer require`, Private Packagist responds with HTTP 410 and a JSON body naming the package and version and explaining that the download was refused. No flagged version can be installed this way on any Composer client, so a Composer 2.4 installation gets the same protection as a current Composer 2.10 installation.

The repository-wide blocking hooks directly into the Aikido malware feed, so a refusal takes effect across your entire Private Packagist repository the moment a version is flagged, with no manual intervention. Packagist.org also removes malware versions after manual review and those deletions propagate to Private Packagist, but that review can take hours, during which the version would otherwise still be installable.

Malware download blocking is enabled by default for all organizations. You can turn it off with the _Block downloads of package versions flagged as malware_ setting, but we recommend leaving it on.

**Please note that this repository-wide download blocking is only effective combined with closed fallback paths.** If Composer cannot get the dist from Private Packagist, it can still fetch the same code from the upstream dist URL. Clients older than Composer 2.10, and Composer 2.10 clients that re-enabled `source-fallback`, can also fall back to a source checkout. To get the full malware protection make sure the following two config options are configured accordingly:

* [Legacy insecure package download fallback](#legacy-insecure-package-download-fallback) setting should be disabled
* [Hide source code checkout URLs from Composer](#hide-source-code-checkout-urls-from-composer) should be enabled for mirrored packages so there is no upstream location left to fall back to. 

See the two sections below for details.

### Why Composer's download fallbacks are a risk

#### Composer download fallback behavior

Composer is built for resilience: if the preferred download location is unavailable, it tries the next one. For a project mirrored through Private Packagist, a single package in `composer.lock` can point at up to three download paths:

1. The preferred mirror URL on Private Packagist.
2. The original upstream dist URL, for example a zip from the GitHub API.
3. The git source repository, cloned as a last resort.

This rarely helps in practice. CI and deployment environments usually hold credentials for Private Packagist only, so when it is unreachable the fallbacks fail on authentication anyway. They succeed silently only for public third-party packages, which is where supply chain risk lives.

#### Supply chain attack example

Suppose a malicious version of `psr/log` is published. Private Packagist detects it and returns a 410 for that version. Without the settings below, Composer treats the 410 as a transient failure, picks the next URL, and downloads the malicious artifact directly from GitHub. If that also fails, it falls back to a source checkout. Every protection Private Packagist applied is bypassed.

On the client side this is governed by `preferred-install` (prefers `dist` since Composer 2.1) and `source-fallback` (defaults to `false` since Composer 2.10). Composer 2.11 removed the `source-fallback` option, and dist installs no longer fall back to source. Because these are client settings, you cannot rely on every environment configuring them securely. The settings below remove the fallback paths from the package metadata itself, so the protection applies regardless of the Composer version or client configuration.

### Legacy insecure package download fallback

Private Packagist replaces all dist/artifact download URLs with its own, so every download goes through Private Packagist and benefits from its access controls, mirroring, and integrity guarantees.

The _Enable legacy insecure package download fallback to original dist file URLs_ setting re-adds the original upstream URL as a dist mirror alongside the Private Packagist URL. When Private Packagist is unavailable or returns errors, Composer can then download the artifact from its original location, bypassing the protections Private Packagist applies.

It is disabled by default for new organizations, and we recommend leaving it disabled unless you have a specific reliability requirement that justifies the trade-off. Organizations created before this setting was introduced on June 1st, 2026 have it enabled by default to preserve their previous behavior. We recommend they disable it too.

After changing this setting, run `composer update mirrors` in your projects to rewrite their lock files so the dist URLs match the selected option.

#### How your lock file changes

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

### Source code checkout URLs

Each version in a Composer repository can advertise a source code checkout URL alongside its dist URLs. The source references a commit in the upstream VCS repository and is persisted in `composer.lock`. Composer uses it as a fallback when the dist URL is inaccessible, bypassing Private Packagist's access controls.

Removing the `source` block prevents this fallback regardless of the Composer version or the client's `preferred-install` and `source-fallback` settings.

#### Options

The _Hide source code checkout URLs_ setting offers:

- **Never**: source checkout URLs are always included when known.
- **Only for packages mirrored from Packagist.org**: hides the source for packagist.org packages only.
- **For all mirrored packages**: hides the source for every third-party mirrored package, while keeping it for your own and private packages.
- **Always**: hides all source code checkout URLs, including your own and private packages.

New organizations default to _For all mirrored packages_, which we recommend. Organizations created before this setting was introduced on June 1st, 2026 default to _Only for packages mirrored from Packagist.org_, and we recommend they switch to _For all mirrored packages_. Keeping the source for your own and private packages lets developers install them from source for local development and contributions. If your developers also contribute to third-party packages from source, _Only for packages mirrored from Packagist.org_ is a useful middle ground.

After changing this setting, run `composer update mirrors` in your projects to rewrite their lock files so the source URLs match the selected option.

#### How your lock file changes

When the source is hidden, the `source` block is dropped and a third-party package keeps only its dist entry:

```json
"dist": {
    "type": "zip",
    "url": "https://repo.packagist.com/my-org/dists/psr/log/abc1234.zip",
    "reference": "abc1234"
}
```

### Composer client version restriction

Older Composer clients carry known vulnerabilities and predate newer protections such as dependency policies and malware handling. Keeping your organization on an up-to-date Composer version ensures it benefits from the latest security features and fixes.

The _Composer client version restriction_ setting controls which Composer client versions may access your organization's Composer repository:

- **All Composer versions allowed**: no restriction.
- **Composer 2.2.\* (LTS) and 2.10.\* (latest)**: only the long-term-support and latest release lines are accepted.
- **Composer 2.10.\* (latest only)**: only the latest release line is accepted.

The allowed release lines are tracked automatically. Private Packagist follows the current LTS and latest Composer release lines as published on Packagist.org, so the versions accepted by the latest and LTS options update as new Composer releases come out; you do not need to change the setting. When a line stops being the LTS or latest line, it stays accepted for a short grace period, so clients on the previous line are not blocked immediately.

The restriction currently matches on the major and minor version only and ignores the patch version, so any patch release within an allowed line (for example `2.10.0` or `2.10.3`) is accepted. This is the first iteration of the setting, and we plan to make it more granular and configurable over time.

New organizations default to _Composer 2.2.\* (LTS) and 2.10.\* (latest)_, which we recommend as a minimum. If your team can stay on the newest release, _Composer 2.10.\* (latest only)_ is better still. Existing organizations keep _All Composer versions allowed_ until you change it. We recommend switching to a more restricted policy once your developers, CI pipelines, and AI coding agents are on a supported version.

The restriction applies only to Composer clients, identified from the request's user agent. All other tooling keeps working as before. When a restricted Composer client tries to fetch metadata or download an archive, the request is refused and Composer shows the reason:

```
COMPOSER VERSION NOT ALLOWED

Your organization restricts the allowed Composer client versions. Please upgrade to one of: 2.2.*, 2.10.*. See https://getcomposer.org/download/ for installation instructions.
```

### Why Composer plugins are a risk

A Composer plugin is a regular package that declares the type `composer-plugin`, or the legacy type `composer-installer`. Composer loads and executes its code during `composer install` and `composer update`. This makes them an attractive target for supply chain attacks. 

Unlike a regular library, plugin code will already run before you can inspect it. And any package can become a plugin: any library can change its type to `composer-plugin` in a new version and execute arbitrary code on every machine that updates to it. This happened on April 30th, 2026, when the compromised `intercom/intercom-php` package registered itself as a plugin and ran a credential-harvesting script during installation.

Composer only executes plugins listed in the [`allow-plugins`](https://getcomposer.org/doc/06-config.md#allow-plugins) configuration of a project's `composer.json`. When run interactively, Composer prompts before adding unknown plugins to that list. Both are client-side mechanisms: they rely on each project's configuration and on each developer making the right call at the prompt. The settings below let you manage allowed plugins for your whole organization instead.

### Composer plugins

#### Limit Composer plugins

The _Composer plugin policy_ setting decides which packages may act as Composer plugins across all of your projects:

- **Allow all**: all plugin packages are served normally.
- **Limit to list below**: only allowlisted packages are served as plugins.

Organizations default to _Allow all_. We recommend _Limit to list below_.

The allowlist takes one package name per line and supports wildcards, e.g. `symfony/*`.

When limited, Private Packagist omits versions with the type `composer-plugin` or `composer-installer` from the metadata of any package that is not allowlisted. Composer never sees those versions and cannot install them. This way you are protected even if `allow-plugins` is misconfigured in a project's composer.json, or a previously trusted dependency suddenly turns itself into a plugin.

Some packages, such as `php-http/discovery`, declare themselves as plugins but are commonly used as regular libraries. They must be allowlisted to be installable at all. Whether a plugin actually executes is still controlled by each project's `allow-plugins` configuration, so a project can use such a package as a library while opting out of running it as a plugin.

#### Block download of disallowed plugin versions

Limiting plugins does not change existing lock files. A `composer install` can still install a disallowed plugin version, because Composer downloads the dist file referenced in `composer.lock` directly. Enable the _Block downloading disallowed plugin versions from existing lock files_ setting to close this gap: Private Packagist then responds with HTTP 410 when Composer requests the dist file of a disallowed plugin version.

Before enabling it, make sure all plugins your projects use are allowlisted. A forgotten entry will surface as an error on the next deploy from an existing lock file.

**Please note that plugin blocking is only effective combined with closed fallback paths.** Otherwise Composer can still fetch a refused version from the upstream dist URL or a source checkout. To get the full protection make sure the following two config options are configured accordingly:

* [Legacy insecure package download fallback](#legacy-insecure-package-download-fallback) setting should be disabled
* [Hide source code checkout URLs from Composer](#source-code-checkout-urls) should be enabled for mirrored packages

### Why a cooldown dependency policy helps

Supply chain attacks often publish a compromised version of a popular package. Such versions are often detected and removed within hours or days. Projects that update during that time install the compromised version. A cooldown policy holds back new versions until they have been public for a while, so most compromised releases are gone before your projects could install them.

### Cooldown dependency policy

The _Cooldown period_ setting holds back package versions that were published within the period. Enter a number and a unit, e.g. `24 hours`, `7 days` or `2 weeks`. The supported units are second, minute, hour, day and week. Leave the field empty to disable the cooldown policy. Organizations have no cooldown policy by default.

Private Packagist leaves versions that are still in the cooldown period out of the metadata it serves to Composer. `composer update` and `composer require` cannot install them and resolve to an older version instead, or fail if no other version matches your constraints. Once the cooldown period has passed, the version appears in the metadata and the next `composer update` can install it. This also works for Composer clients that have cached the metadata.

The version list on the package page greys out versions that are still in the cooldown period. Hover over a version to see when it becomes available to Composer.

The cooldown is measured from the publication time Private Packagist serves as `published-time` for each version. The following versions are never held back:

- Dev versions such as `dev-main`
- Versions of packages listed under _Packages without cooldown_

`composer install` from an existing lock file is not affected. Private Packagist still serves the dist files of versions in the cooldown, so a lock file created before a version was held back keeps working.

Composer 2.11 can also apply a [cooldown](https://getcomposer.org/doc/06-config.md#cooldown) configured in each project. The Private Packagist cooldown policy applies to all projects that use your organization's Composer repository, independent of the Composer version and the project configuration. You can use both together.

#### Packages without cooldown

The _Packages without cooldown_ list takes one package name per line and supports wildcards, e.g. `acme/*`. Versions of these packages are never held back. Add your own packages, or a package you need to update right away, for example to install a security fix.
