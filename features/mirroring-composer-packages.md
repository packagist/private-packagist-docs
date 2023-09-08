# Mirroring Composer Packages
## Redundancy and Dependency Integrity with Private Packagist

When you first run Composer, you usually install some open-source dependencies from its default package archive [packagist.org](https://packagist.org). Packagist.org is the public repository for all open-source PHP packages. So when you add an open-source dependency to your project Composer fetches its metadata, its description, a list of versions, requirements, etc., from Packagist.org.

## Installing packages from Packagist.org
**Packagist.org only collects and serves package metadata.** Most importantly the names of packages, their respective dependencies and the location of their source code. Once Composer resolved the set of dependencies it writes out the _composer.lock_ file containing all metadata for versions of packages that need to be installed. Composer then downloads every package listed in _composer.lock_. For each package **Composer** either **downloads** a **distribution file** (zip or tar) or **clones** the respective **version control system** (git, hg or svn) **defined by the package maintainer**. By default it selects a distribution file for every tagged release, you can modify Composer’s preference with the `--prefer-source` and `--prefer-dist` options.

This means that **installing open-source packages from Packagist.org relies on the respective package maintainers’ source code hosting to be up and running whenever you run _composer install_**. Most of the time that’s GitHub but it may be any other service or even hosted by the package maintainer themselves. Packagist.org doesn’t handle building archives, storing or distributing the package source code.

## Removing the single point of failure for installs
When using Composer with **Private Packagist, Composer will store two download locations** for every package in your composer.lock: the Private Packagist mirror URL and the original download URL. Composer will download the Private Packagist mirror of distribution files which gives you **faster downloads**. But more importantly **even if GitHub, Bitbucket, GitLab or an open-source maintainers self-hosted version control system are down, _composer install_ can still install your dependencies!** So if you rely on _composer install_ in your build process it no longer depends on these services being available. But better yet, since Composer stores both URLs this **protects you from any Private Packagist downtime too!** There is **no longer any single point of failure**.

Further, mirroring gives you a copy of all dependencies your production system requires, so even **if an open-source maintainer deletes their project** you can safely switch to a new package at your own pace, because **Composer can still install it** from your Private Packagist repository until you remove it, too.

## Mirrored Repositories on Private Packagist
When you create a new organization on Private Packagist, it is **automatically set up to mirror** all your dependencies from the open-source package archive **Packagist.org**. But you can mirror any number of public or private repositories, e.g. the Drupal package repository or Magento Marketplace (see “[Mirroring Magento Marketplace Packages](./../docs/mirror-magento-marketplace.md)”).

![Packagist.org Mirroring](/Resources/public/img/docs/features/Packagist.org-Mirror-20230908.png)

By default packages are automatically mirrored and added to your Private Packagist repository the first time they are accessed through composer update. Automated systems using Private Packagist access tokens cannot mirror new packages to ensure that build processes do not have unintended consequences.

![Packagist.org Mirroring](/Resources/public/img/docs/features/Packagist.org-Mirror-Edit-20230908.png)

You can configure the mirroring policy on a per-repository basis. For example you can ensure new open-source dependencies are discussed or reviewed before they are manually added by an administrator, making them available to all developers in the organization.
