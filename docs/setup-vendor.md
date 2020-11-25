# Vendor's Customers using Private Packagist in a Composer project

Using Private Packagist for Vendors, your customers can install packages with Composer using a unique URL and an authentication token.

Customers can start using your packages in either an existing or a new Composer project.

## Adding a Package to an existing Composer project

To install a package named "acme-company/api" into an existing project, The following steps are required:

1. Configure authentication. The command to set an authentication token is listed on the customer Composer information page.
2. Add the customer repository in the project's composer.json. The customer repository URL is listed on the customer Composer information page. The project's composer.json should now have a new entry:
    ```
    "repositories": [
        {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
    ]
    ```
3. Install the package by running `composer require acme-company/api`
           
![Customer Composer Information](/Resources/public/img/docs/customer-composer-information.png)

## Starting a new Composer Project

To set up a new project, your customers can initialize the project with the Customer's Composer repository URL or use a template package.

### composer init
To initialize a new Composer project the following steps are required:

1. Configure authentication. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer init --name=acme/website --repository=https://acme.repo.packagist.com/customer/` . The customer repository URL is listed on the customer Composer information page. 

### composer create-project

To initialize a project using a template package named `acme/website`, then these steps are required:

1. Configure authentication. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer create-project acme/website --add-repository --repository=https://acme.repo.packagist.com/customer/` . The customer repository URL is listed on the customer Composer information page. 

Once the command is finished, the created composer.json file will contain the added repositories:

```
"repositories": [
    {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
]
```

*Important Note:* While using the `add-repository` flag and in case a composer.lock file is present, the lock file will be deleted and an update will be run instead of install.
