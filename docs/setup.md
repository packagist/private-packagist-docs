# Composer Project Setup

You can use Private Packagist in a new or in an existing Composer project. Some of the steps in this guide will only apply if your project needs the respective Composer feature explained in that step.

## Creating a new Composer project with Private Packagist

When setting up a project it's important to add both the private repository URL for your Private Packagist organization and to disable packagist.org. Disabling the public open-source repository packagist.org is important because Private Packagist mirrors all the packages you get from there. If you do not disable it, Composer v1 will load all public packages twice which will slow down Composer and use more memory. Composer v2 automatically skips packagist.org packages if they are mirrored through Private Packagist.

### composer init
You can initialize a new Composer project using your Private Packagist repository by adding arguments to the `composer init` command.

1. Configure authentication on your machine. The command to set an authentication token is listed on the organization overview.
2. Assuming the organization URL name is `acme-company` run the command `composer init --repository="https://repo.packagist.com/acme-company/" --repository='{"packagist.org": false}'`

### composer create-project
If you would like to use the `create-project` command to initialize a project using a template package named `acme/website` in your Private Packagist repository then these steps are required:

1. Configure authentication on your machine. The command to set an authentication token is listed on the organization overview.
2. Run `composer create-project acme/website --add-repository --repository='{"packagist.org": false}' --repository="https://repo.packagist.com/acme-company/"`. This will create a new composer project for you.

Once the command is finished, the created composer.json file will contain your organization's repository URL and have packagist.org disabled so mirroring is fully used.

```
"repositories": [
    {"type": "composer", "url": "https://repo.packagist.com/acme-company/"},
    {"packagist.org": false}
]
```

Alternatively, it is possible to configure your template package to already include your Private Packagist repository in its composer.json. We do not encourage this approach, because this pre-defined repository
will cause conflicts if you rely on functionality that uses different private repositories, such as suborganizations or vendor customers. If you do prefer to take this approach, you should remove the `--add-repository` option from the `create-project` command.  

Note: Running `composer create-project` with multiple `--repository` arguments is only supported with Composer 2.

## Adding Private Packagist to an existing project with a composer.json

### Sample composer.json before moving to Private Packagist

To illustrate the process we'll refer to the following example Composer project *acme/website* in the next sections.

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

### Basic Setup

Three steps are required to use Private Packagist on an existing project. They are also documented on the overview page of your organization.

1. You need to configure authentication on your machine. Every developer on your team must do this, so they can access code stored in Private Packagist.
    ```
    composer config --global --auth http-basic.repo.packagist.com your-username your-secret-token-here
    ```
    Alternatively, for example for CI environments, you can use an environment variable:
    ```
    COMPOSER_AUTH='{"http-basic":{"repo.packagist.com":{"username":"token","password":"your-authentication-token-here"}}}'
    ```
2. You need to tell Composer where to look for your packages. So you have to add Private Packagist as a repository to your composer.json file and you need to disable packagist.org because public packages will be mirrored through Private Packagist, too.
    ```
    "repositories": [
        {"type": "composer", "url": "https://repo.packagist.com/acme-company/"},
        {"packagist.org": false}
    ]
    ```
3. Run `composer update mirrors` to mirror all dependency packages into Private Packagist. We recommend you delete your local vendor directory before running this command to avoid any issues with cached URLs, see below for an explanation.

After these three steps Composer commands will access Private Packagist but it will only use it to download mirrored packages from packagist.org.

The following additional steps in this document will help you get the full benefit out of Private Packagist like accessing your private code and mirrored packages from other sources.

#### Delete your vendor directory

If you are using Composer v1 we recommend you delete your vendor directory **before** you migrate your project to Private Packagist.
Composer v1 releases sometimes use data found in your vendor directory instead of fetching fresh data.
If this happens during the initial “composer update mirrors” run it can prevent mirror and download URLs from being updated.
Packages will then be downloaded from the old location, so you would not be using Private Packagist's more reliable downloads and CDN.
It can also prevent notification URLs from updating, in which case your install statistics will be inaccurate.

## Add VCS repositories (Git, Hg/Mercurial, SVN/Subversion)

The sample composer.json file for *acme/website* contains two VCS repositories

- a fork of twig/twig at `https://github.com/acme-website/twig`. This fork contains changes you need in your project only
- a GitHub repository at `https://github.com/acme-website/custom-wordpress-bundle` which contains a private package used only in your company

Without Private Packagist, every time you run `composer update` Composer loops through all tags and branches of every VCS repository to identify which versions may be installable.
It then checks whether the branch/tag contains a valid composer.json.
Especially for repositories with lots of tags or branches this can take a long time.

You can add repositories to Private Packagist on the Packages page by clicking on _Add Package > By Url_, selecting the type _VCS_ and entering its URL. For private repositories requiring authentication you need to select a credential as explained in the next section.

If you add a repository hosted on GitHub, GitLab, Bitbucket or Bitbucket Server using a credential, then Private Packagist will automatically set up a webhook to get notified about new commits and versions. Therefore it is beneficial to use a credential even if the repository is publicly available. Repositories where no webhook can be set up will only be updated once every 12 hours.

### Private VCS repository requiring authentication

For Private Packagist to be able to access repositories that require authentication, e.g. a private GitHub repository, we first need to set up a credential under _Settings > Stored Credentials_. Once the credential is created the repository can be added on the Packages page via *Add Package > By Url*. 

Note: Even though you need to select a domain when you create a credential Private Packagist will not automatically apply the credential for all HTTP calls to that domain. Private Packagist will only suggest it where it might be useful. Therefore it is important that you explicitly select it when you add the credential or else Private Packagist will not be able to import the repository.

### General webhook setup

Webhooks are automatically configured for packages from GitHub, GitLab, and Bitbucket when added through a synchronization or added by URL with a credential with permissions to configure a webhook.

See integration specific information on webhooks here: [Integrations](/features/integration-github-bitbucket-gitlab)

For other code-hosting platforms or custom setups, you can also configure the webhook manually.

Find your unique webhook URL on the package details page:

![Webhook Setup](/Resources/public/img/docs/integration-setup/webhook-setup.png)

Configure your webhook to trigger on the following repository events:
- On each pus
- When a branch is created
- When a branch is deleted
- When a tag is created
- When a tag is deleted

These events ensure your package metadata stays synchronized with your repository changes, enabling automatic updates when you publish new versions or modify your codebase.

### Add a forked VCS repository

When adding a fork to Private Packagist it can happen that the original package was already added. Since Composer cannot load multiple packages by the same name from one URL, the new package will be marked as uninstallable with Composer due to a conflicting name. You will have to delete the original package and then add your fork. Once the changes of the fork are merged back upstream and you are ready to delete the fork you can also delete the package in Private Packagist. Private Packagist will then automatically mirror the original package from packagist.org again.

## Add a custom package

Custom package definitions in your composer.json can be added via _Add Package > Custom Package_. In the textarea you are then able to paste the entire package definition and select a credential if one is necessary to access the zip files.

Example:
```json
{
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
```

Alternatively you may submit an array of packages to define multiple versions:

```json
[
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
```
Once a custom package has been added to Private Packagist you will benefit from mirroring the zip file. Composer will have an additional location to download the file from if the original storage becomes unavailable.

### Prevent a package from being used with custom packages

While there is no direct "block package" feature in Packagist, you can effectively prevent a specific package from being used in your project by creating a placeholder package. 

The package type [metapackage](https://getcomposer.org/doc/04-schema.md#type) ensures no code is associated with it. Optionally, you can mark the package as [abandoned](https://getcomposer.org/doc/04-schema.md#abandoned) and, if needed, suggest an alternative. 

```json 
{
  "name": "acme/blocked-package",
  "version": "0.0.1",
  "type": "metapackage", 
  "abandoned": "acme/other-package"
}
```

If necessary, make sure the package is added to all relevant suborganizations. 
 
This placeholder package will prevent any other package with the same name from being mirrored automatically, effectively blocking the problematic package from being used in your projects.

## Add an artifact package

You can upload code archives via _Add Package > Artifact_. Your uploaded archives need to contain a valid composer.json file in its root directory and must be of type zip, gz, or bz2. Once you upload your code archives, you can save the artifact package and use it in your organization.

## Import from an existing Satis instance

Once you start using Private Packagist it usually also replaces your previous Satis setup. Via _Add Package > Json Import_ you can paste your satis.json into the textarea and select which credentials should be used to try to import the packages. This will add all packages from your Satis instance to Private Packagist and will make Satis obsolete and therefore one less thing you have to worry about.

## Add mirrored third party repositories
On your organization's settings page under "Mirroring" you can add additional third party mirrored repositories. By default packagist.org is enabled for all organizations so there is no need to set that up.

Similar to how Private Packagist can access private VCS repositories it can also access private mirrored third party repositories e.g. [repo.magento.com](mirror-magento-marketplace). First create the credentials that are necessary e.g. your Magento Marketplace access keys and then add the mirrored third party repository. Make sure to select the credential while creating the mirror. All packages added from the repository will now automatically use that credential.

If you are moving one of the additional Composer repositories to Private Packagist after you already started to use Private Packagist, make sure to run `composer update mirrors` again to update the download locations for packages from the mirror in your lock file.

**Important:** Third party repositories do not provide a way for us to get notified whenever new versions of a package appear. Therefore packages which have been added via mirror will only be updated once every 12 hours.

For more information on permission levels, managing repositories, and troubleshooting, see [Mirrored Third-Party Repositories](mirrored-repositories).

## Cleanup
Once all the repository entries have been added to Private Packagist the setup is complete and you can remove all the repositories so it will look exactly like what you see in the snippet on your Organization’s overview page.

## The composer.json file for acme/website after applying all steps

```json
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

