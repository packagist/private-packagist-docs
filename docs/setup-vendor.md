# Getting started with Private Packagist for Vendors in a Composer project

Using Private Packagist for Vendors, customers install packages with Composer using a unique URL and an authentication token.

Customers can start using Private Packagist in either an existing or a new Composer project.

## Adding a Package from a vendor to an existing Composer project

To install a package named "acme-company/api" into your existing project, you need to follow the steps below:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Add the customer repository in your project's composer.json. You can find the  customer repository URL on the customer Composer information page. Your project's composer.json should have a new entry:
    ```
    "repositories": [
        {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
    ]
    ```
3. Install the package by running `composer require acme-company/api`
           
![Customer Composer Information](/Resources/public/img/docs/customer-composer-information.png)

## Starting a new Composer Project

To set up a new project, you can initialize the project with the Customer's Composer repository URL or use a template package.

### composer init
You can initialize a new Composer project by following the required steps:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer init --name=acme/website --repository=https://acme.repo.packagist.com/customer/` . You can find the customer repository URL on the customer Composer information page. 

### composer create-project

If you would like to initialize a project using a template package named `acme/website`, then these steps are required:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer create-project acme/website --add-repository --repository=https://acme.repo.packagist.com/customer/` You can find the customer repository URL on the customer Composer information page. 

Once the command is finished, the created composer.json file will contain the added repositories:

```
"repositories": [
    {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
]
```

*Important Note:* While using the `add-repository` flag and in case a composer.lock file is present, the lock file will be deleted and an update will be run instead of install.
