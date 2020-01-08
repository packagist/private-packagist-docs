# How to move a Project to Private Packagist
This guide will walk you through all necessary steps to set up Private Packagist for your composer project. It contains an initial list of three steps which are required for every project to integrate with Private Packagist and an additional set of steps which will only apply to some projects.

### This is what we start with

```json
{
    "name": "acme/website",
    "repositories": [
        {"type": "vcs", "url": "https://github.com/acme-website/twig"},
        {"type": "vcs", "url": "https://github.com/acme-website/custom-wordpress-bundle"},
        {"type":  "composer", "url":  "https://repo.magento.com"},
        {"type":  "composer", "url":  "https://satis.acme-company.com"},
        {
            "type": "package",
            "package": {
                "name": "smarty/smarty",
                "version": "3.1.7",
                "dist": {
                   "url": "https://www.smarty.net/files/Smarty-3.1.7.zip",
                   "type": "zip"
                },
                "source": {
                   "url": "http://smarty-php.googlecode.com/svn/",
                   "type": "svn",
                   "reference": "tags/Smarty_3_1_7/distribution/"
                },
                "autoload": {
                   "classmap": ["libs/"]
                }
            }
        }
    ],
    "license": "proprietary",
    "autoload": {
        "psr-4": { "Acme\\": "src/Acme/"}
    },
    "autoload-dev": {
        "psr-4": { "Acme\\": "tests/" }
    },
    "require": {
        "php": "^5.6",
        "acme/custom-wordpress-bundle": "dev-master",
        "monolog/monolog": "^1.24.0",
        "twig/twig": "^2.8.1",
        "doctrine/orm": "^2.6",
        "smarty/smarty": "3.1.7"
    },
    "require-dev": {
        "phpunit/phpunit": "^8.1",
        "phpstan/phpstan": "^0.11.4"
    }
}

```

### Add Private Packagist to your composer.json

To make your project work with Private Packagist there are three steps required which are documented on the overview page of your organization.

1. You need to configure authentication on your machine. This is the step that is required for every developer interacting with composer.
2. In your composer.json Private Packagist needs to be added as a repository and packagist.org needs be disabled as shown in the snippet.
3. Run `composer update mirrors`. This will add all dependencies as packages in Private Packagist. To avoid any issues with outdated mirror urls or notification urls we also recommend that you delete your local vendor directory.

Once these three steps are done then running composer install/update will go against Private Packagist but there are additional steps which are required to get the full benefit out of Private Packagist.

##### Creating a new composer project instead

If instead of migrating an existing composer project you are creating a new composer project where you would like to use Private Packagist then these two steps are required:

1. You need to configure authentication on your machine.
2. Run `composer create-project --repository-url=https://repo.packagist/acme-company/`. This will create a new composer project for you and add Private Packagist as repository. Once the command is finished you will still have to add `{"packagist.org": false}` to your composer.json.

##### Delete your vendors directory
As already mentioned, we recommend that you delete your vendor directory when you migrate your project to use Private Packagist. The reason for this is that composer in some cases uses existing data from your vendor directory instead of the newly fetched data. If this happens during the initial “composer update mirrors” run then it can lead to mirror urls not being updated which means that packages will be downloaded from the wrong mirror or it can lead to notification urls not being updated which means that your install counts will be inaccurate.

### Add VCS repository

The sample composer.json file contains two VCS repositories: a fork of twig/twig that contains changes for which a pull request hasn't been created yet and a GitHub repository which contains a private package. Every time a composer update is run, composer loops through all tags and branches of every VCS repository. It then checks whether the branch/tag contains a validate composer.json. Especially for repositories with lots of tags and/org branches this can take quite some time.

If you add a repository hosted on GitHub, GitLab, Bitbucket and Bitbucket Server with a credential Private Packagist will also automatically set up a webhook to get notified about new commits and versions. Therefore it is beneficial to use a credential even if the repository is publicly available. Repositories where no webhook can be set up will only be updated once every three hour.

##### Add a private VCS repository

For Private Packagist to be able to access repositories that require authentication, e.g. a private GitHub repository, we first need to set up a credential under “Settings” -> “Stored Credentials”. Once the credential is created the repository can be added on the Packages page via “Add Package” -> “By Url”. 

Note: Even though you need to select a domain when you create a credential Private Packagist will not automatically apply the credential for all http calls to that domain. Private Packagist will only suggest it where it might be useful. Therefore it is important that you explicitly select it when you add the credential or else Private Packagist will not be able to import the repository.

##### Add a forked VCS repository

When adding a fork to Private Packagist it can happen that the original package was previously added. Every organization can only have the same package name once which means that adding a fork when the original package is already added will fail. You will have to delete the original package first and then add your fork. Once the changes of the fork are merged back upstream and you are ready to delete the fork you can also delete the package in Private Packagist. Private Packagist will then automatically fallback to the original package from packagist.org

###[Add a custom package](#custom-json-package)

Custom package definitions in your composer.json can be added via "Add Package" -> "Custom Package". In the textarea you are then able to paste the entire package definition and select a credential if one is necessary to access the zip files.

Example:
```json
{
    "type": "package",
    "package": {
        "name": "smarty/smarty",
        "version": "3.1.7",
        "dist": {
            "url": "https://www.smarty.net/files/Smarty-3.1.7.zip",
            "type": "zip"
        },
        "source": {
            "url": "http://smarty-php.googlecode.com/svn/",
            "type": "svn",
            "reference": "tags/Smarty_3_1_7/distribution/"
        },
        "autoload": {
            "classmap": ["libs/"]
        }
    }
}
```

The "package" key in a package repository may be set to an array to define multiple versions of a package:

```json
{
    "type": "package",
    "package": [
        {
            "name": "foo/bar",
            "version": "1.0.0",
            ...
        },
        {
            "name": "foo/bar",
            "version": "2.0.0",
            ...
        }
    ]
}
```
Once a custom package has been added to Private Packagist it will also benefit from us mirroring the zip file and provide additional endpoint to download the file in case the original storage becomes unavailable.

### Add your Satis instance

Once you start using Private Packagist it usually also replaces your previous Satis set up. Via "Add Package" -> "Json Import" you can paste your satis.json into the textarea and select which credentials should be used to try to import the packages. This will add all packages from your Satis instant to Private Packagist and will make Satis obsolete and therefore one less thing you have to worry about.

### Add a mirrored third party repositories
On your organisation’s settings page under “Manage Mirrored Repositories” you can add additional third party mirrored repositories. By default packagist.org is enabled for all organisations so there is no need to set that up.

Similar to how Private Packagist can access private VCS repositories it can also access private mirrored third party repositories e.g. repo.magento.com. First create the credentials that are necessary e.g. your Magento Marketplace access keys and then add the mirrored third party repository. Make sure to select the credential while creating the mirror. All packages added from the repository will now automatically use that credential.

Important note: third party repositories do not provide a way for us to get notified whenever new versions of a package appear. Therefore packages which have been added via mirror will only be updated once every three hours.

### Cleanup
Once all the repositories entries have been added to Private Packagist the set up is fully complete and you can remove all the repository so it will look exactly like what you see in the snippet on your Organization’s overview page.

### This is how you composer.json looks like

{
    "name": "acme/website",
    "repositories": [
        {"type":  "composer", "url":  "https://repo.packagist.com/acme-company"},
        {"packagist.org": false}
    ],
    "license": "proprietary",
    "autoload": {
        "psr-4": { "Acme\\": "src/Acme/"}
    },
    "autoload-dev": {
        "psr-4": { "Acme\\": "tests/" }
    },
    "require": {
        "php": "^5.6",
        "acme/custom-wordpress-bundle": "dev-master",
        "monolog/monolog": "^1.24.0",
        "twig/twig": "^2.8.1",
        "doctrine/orm": "^2.6",
        "smarty/smarty": "3.1.7"
    },
    "require-dev": {
        "phpunit/phpunit": "^8.1",
        "phpstan/phpstan": "^0.11.4"
    }
}

```
