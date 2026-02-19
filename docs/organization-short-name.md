# Organization short name
## 

The organization short name is used in your Private Packagist's Composer repository URL. 

For example, if your organization short name is `acme-inc`, your Private Packagist's Composer repository URL will be `https://repo.packagist.com/acme-inc/`. 

## Renaming

You can rename the organization short name to change your organization's repository URL on your organization's _Settings > General_ page.   

When you change this short name, Composer projects referring to the current URL will no longer be able to install packages. You'll have to update their composer.lock files by following these steps:

1. Update the Private Packagist repository URL in composer.json 
2. Run `composer update --lock` to update your composer.lock file with the new repository URL

