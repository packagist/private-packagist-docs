# How to move a Project to Private Packagist
This guide will walk you through all steps to set up Private Packagist for your composer project. Depending on your composer.json some steps might not be necessary.

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

To make you project work with Private Packagist there are three steps required which are documented on the overview page of your organization.

1. You need to configure authentication on your machine. This is the step that is required for every developer interacting with composer.
2. In your composer.json Private Packagist needs to be added as a repository and packagist.org should be disabled as shown in the snippet.
3. Run `composer update mirrors`. This will add all dependencies as packages in Private Packagist. To avoid any issues with outdated mirror urls or notification urls we also recommend that you delete your local vendor directory.

Once these three steps are done then you running composer install/update will run against Private Packagist but there are some more steps that are required to get the full benefit out of it. Leaving it in the current step will not let you from the performance benefits that you get from using Private Packagist. 

##### Delete your vendors directory
As already mentioned we recommend that you delete your vendor directory when you migrate your project to use Private Packagist. The reason for this is that composer in some cases uses existing data from your vendor directory instead of the newly fetched data. If this happens during the initial “composer update mirrors” run then it can lead to mirror urls not being update which means that packages will be downloaded from the wrong mirror or it can lead to notification urls not being updated which means that your install counts will be inaccurate.

### Add VCS repository

The sample composer.json file contains two vcs repositories: a fork of twig/twig that contains custom enhancements for which a pull request hasn't been created yet and a GitHub repository which contains a private package. Every time a composer update is run composer loops through all tags and branches of the two repositories. This can take quite some time.

##### Add a private VCS repository which requires credentials

For Private Packagist to be able to access repositories that require authentication e.g. the private GitHub repository we first need to set up a credential under Setting -> Stored Credentials. Once the credential is created the repository can be added on the Packages page via Add Package -> By Url. It is important that the credential is selected or else Private Packagist will not be able to import the repository. Repositories hosted on GitHub, GitLab, Bitbucket and Bitbucket Server Private Packagist will also set up a webhook to get notified about new commits and versions.

##### Add a VCS repository without credentials

Importing a public repository does not require setting up credentials. But adding credentials for packages that are hosted on GitHub, GitLab or Bitbucket comes with the benefit that Private Packagist is able to set up webhooks which then notify us about new or updated versions on every push.

When importing the fork it can happen that the original package was previously added to Private Packagist. Every organization can only have the same package name once which means that in this case adding a fork will fail and you will have to delete the original package first before you add your fork. Once the changes of the fork are merged back upstream and you are ready to delete the fork you can also delete the package in Private Packagist. Private Packagist will then automatically fallback to the original twig/twig package from packagist.org

### Add a custom package

Custom package definitions in your composer.json can be added via "Add Package" -> "Custom Package". In the textarea you are then able to paste the entire definition and select a credential if one is necessary to access the zip file.

### Add your Satis instance

If you have been using Satis before then Private Packagist also provides a way to import all your packages from your Satis instance at once via "Add Package" -> "Json Import".  Paste your satis.json into the textarea and select which credentials should be used to try to import the packages.

### Add a mirrored third party repositories
On your organisation’s settings page under “Manage Mirrored Repositories” you can add additional third party mirrored repositories. By default packagist.org is enabled for all organisations so there is no need to set that up again.

Similar to how Private Packagist can access private vcs repositories it can also access private mirrored third party repositories e.g. repo.magento.com. First create the credentials that are necessary e.g. your Magento Marketplace access keys and then add the mirrored third party repository. Make sure to select the credential while creating the mirror. All packages added from the repository will now automatically use that credential.

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
