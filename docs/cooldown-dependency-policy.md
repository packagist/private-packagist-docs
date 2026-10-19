# Cooldown dependency policy
## 

Composer 2.11 can hold back package versions that were published recently. Malicious versions are often found and removed within hours, so waiting up to a day before installing a new version reduces the risk of installing such malware. Composer calls this the [cooldown policy](https://getcomposer.org/doc/06-config.md#cooldown).

Composer calculates each version's cooldown period from the publication time that Private Packagist provides to Composer for that version.

## Enable the cooldown policy

Set a cooldown period in the `config` section of your project's composer.json:

```json
{
    "config": {
        "policy": {
            "cooldown": {
                "period": "1 day"
            }
        }
    }
}
```

The cooldown policy applies when Composer chooses versions, during `composer update` and `composer require`. It does not apply to `composer install`, because the versions in your lock file were already chosen. Dev versions, such as `dev-main` for the branch `main`, are never held back because they are moving targets.

To hold back new versions for all projects that use your organization's Composer repository, independent of their composer.json and Composer version, configure a [cooldown dependency policy](security-settings#cooldown-dependency-policy) under _Settings > Security_. It uses the same publication time.

## Release date and publication time

A version's release date is often not the same as its publication time, which is then used for the cooldown period. To explain the relationship, Private Packagist takes a version's release date, the `time` in its metadata, from these sources:

- **Tags from VCS repositories:** the `time` field in the composer.json of the tagged commit. Without one, the date of the tagged commit.
- **Mirrored packages:** the `time` field in the mirrored repository's metadata
- **Custom packages:** the `time` field in the package definition, which defaults to the time you save the custom package JSON definition.
- **Artifact packages:** the time the file was uploaded

Anyone who creates a version can choose its release date, for example with a `time` field or a backdated commit. The publication time is a separate value that Private Packagist sets. It is designed to be more difficult for an attacker to manipulate. So the publication time can differ from the release date:

- A version that appears after you added a package uses the time when Private Packagist imports the version for the first time as its publication time, even if its release date is older/backdated. Private Packagist ignores a `published-time` in the package's own composer.json. If a mirrored third-party repository serves a publication time, Private Packagist trusts the mirror's publication time instead, see [Mirrored third-party repositories that serve a publication time](#mirrored-third-party-repositories-that-serve-a-publication-time).
- Once a version has a publication time, it never moves to an earlier date.

The package page on Private Packagist and `composer show` display the release date, not the publication time.

## How the publication time is set

Private Packagist serves a publication time for every version of every package.

### Mirrored third-party repositories that serve a publication time

If a mirrored third-party repository serves a `published-time` for a version, as Packagist.org does, Private Packagist uses it. The mirrored third-party repository decides when the cooldown period starts for its versions. When a misbehaving mirrored third-party repository modifies the publication time for a version after the fact, it is only accepted by Private Packagist, if it points to a later time than the original time. This behavior essentially allows a mirrored third-party repository to restart or extend the cooldown period for a version, but it does not allow them to reduce or skip the cooldown period after the fact.

Private Packagist trusts a mirrored third-party repository's publication time the same way it trusts the code the repository serves. If a mirrored third-party repository lets package authors set the publication time, for example by copying it from the package's composer.json, an author can backdate a new version so it skips the cooldown period. Only rely on the cooldown policy for packages from such a repository if you trust their authors.

### All other packages

For all other packages, the publication time depends on whether a version already existed when you added the package to Private Packagist:

| Package | Versions that exist when the package is added | Versions that appear later                     |
|---|---|------------------------------------------------|
| Packages from VCS repositories | The release date | The time Private Packagist imports the version |
| Mirrored packages without a publication time | The release date, or the time Private Packagist imports the version if the mirrored repository serves no date | The time Private Packagist imports the version |
| Custom packages | The release date | The time Private Packagist saves the version   |
| Artifact packages | The time the file was uploaded | The time the file was uploaded                 |

### When an existing version changes

For all packages, except packages from mirrored third-party repositories that serve a publication time, the publication time of an existing version depends on what changed:

| Change | Publication time |
|---|---|
| The version points to different code: a new commit, or a dist file with a different checksum | Updated to time Private Packagist imports the change |
| The source or dist URL changes, and the version still points to the same git or Mercurial commit or to a dist file with the same checksum | Unchanged |
| The source or dist URL changes otherwise | Updated to time Private Packagist imports the change |
| The release date or other metadata changes | Unchanged |

Whenever the publication time moves to a later date, the cooldown period starts again for that version. Until it has passed, `composer update` installs an older release instead, or fails if no other version matches your requirements.

A version that is deleted and added again, for example a tag that is deleted and pushed again, gets the time Private Packagist imports it again. If it comes from a mirrored third-party repository that serves a publication time, it gets the repository's value.

Private Packagist replaces a publication time in the future with the time it imports the version.

## When the cooldown policy holds back a version you need

A version can be held back longer than you expect:

- A version whose code changed, for example a force-pushed tag, starts its cooldown period again, see [When an existing version changes](#when-an-existing-version-changes).
- Versions of a newly added mirrored package that have no date in the mirrored repository get the time Private Packagist imports them.
- A tag that could not be imported when the package was added, for example because of an invalid composer.json or a temporary error at the code hosting platform, uses the time Private Packagist imports it once the problem is resolved. _View Log_ on the package page shows such errors.
- New versions of packages that Private Packagist does not update automatically can take several hours to [appear](troubleshooting#new-versions-do-not-appear-immediately), and their cooldown period starts only then. This affects VCS packages without a webhook and packages from mirrored third-party repositories that do not publish [which packages changed](https://packagist.org/apidoc#track-package-updates). The webhook status on the package list shows which packages are updated automatically. Use the update button on the package page to import a new version earlier.

Composer prints how many versions it held back. Run the command with `-vv` to list them with their publication time and when each one becomes available. To install a held-back version earlier:

- Update only this package with the cooldown policy disabled, so all other packages keep their cooldown period: `COMPOSER_POLICY_COOLDOWN_PERIOD=0 composer update acme/package`
- Exclude packages permanently with the `ignore` setting. An entry can be limited to a version constraint, see Composer's [ignore format](https://getcomposer.org/doc/06-config.md#ignore-format).

If you trust the code of your own packages, you can exclude your vendor namespace:

```json
{
    "config": {
        "policy": {
            "cooldown": {
                "period": "1 day",
                "ignore": {
                    "acme/*": "Our own packages"
                }
            }
        }
    }
}
```

If your organization's [cooldown dependency policy](security-settings#cooldown-dependency-policy) holds back the version, Private Packagist leaves it out of the metadata, so Composer does not list it as held back and the options above do not install it. Add the package to _Packages without cooldown_ under _Settings > Security_ instead.

To avoid restarting the cooldown period for your own packages, release a new version instead of force-pushing an existing tag.

## See also

- [Security settings](security-settings#cooldown-dependency-policy) - Hold back new versions for all projects of your organization
- [Package lifecycle](package-lifecycle) - How Private Packagist handles new, changed, and deleted versions
- [Mirrored third-party repositories](mirrored-repositories) - Adding and managing mirrored third-party repositories
- [Troubleshooting](troubleshooting) - When new versions do not appear
