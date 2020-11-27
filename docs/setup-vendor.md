# Vendor's Customers using Private Packagist in a Composer project

Using Private Packagist for Vendors, your customers can install packages with Composer using a unique URL and an authentication token. You can create customers and manage their package access customers on the vendor page in your organization.

Customers can use your packages in either an existing or a new Composer project.

## Adding a package to an existing Composer project

To install a package named "acme-company/api" into an existing project, the following steps are required:

1. Configure authentication: The command to set an authentication token is listed under the vendor tab on the customer's "Composer Instructions" page.
2. Add the customer repository in the project's composer.json. The customer repository URL is listed under the vendor tab on the customer's "Composer Instructions" page. The project's composer.json should now have a new entry (assuming your organization is called `acme` and your customer is called `customer`):
    ```
    "repositories": [
        {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
    ]
    ```
3. Install the new package by running `composer require acme-company/api`
           
![Customer Composer Instructions](/Resources/public/img/docs/customer-composer-instructions.png)

## Starting a new Composer project

To set up a new project using your private packages, your customers can initialize an empty project with their unique Composer repository URL or copy a template project package from your Private Packagist repository.

### composer init
To initialize a new empty Composer project the following steps are required:

1. Configure authentication: The command to set an authentication token is listed under the vendor tab on the customer's "Composer Instructions" page.
2. Run `composer init --name=my/website --repository=https://acme.repo.packagist.com/customer/` . The customer specific URL is listed under the vendor tab on the customer's "Composer Instructions" page. Please replace "acme" with your own organization name, and "customer" with the customer's name. The package name "my/website" can be freely chosen by the customer.

After these steps composer require commands in the project can now access packages in your Private Packagist repository. 

### composer create-project

If you wish to provide a template package for new projects to your customers, the following steps show how to initialize a project from an example template package named `acme/website`. Your template project should not contain a Private Packagist repository URL in its composer.json, so the customer's own URL can be added while creating a new project.

1. Configure authentication: The command to set an authentication token is listed under the vendor tab on the customer's "Composer Instructions" page.
2. Run `composer create-project acme/website --add-repository --repository=https://acme.repo.packagist.com/customer/` . The customer repository URL is listed under the vendor tab on the customer's "Composer Instructions" page.  

Once the command is finished, the created composer.json file will contain the customer's Private Packagist repository URL:

```
"repositories": [
    {"type": "composer", "url": "https://acme.repo.packagist.com/customer/"}
]
```

*Important Note:* If there is a composer.lock in your template project, the `add-repository` option will delete the lock file and run an update to generate a new one using the customer's personalized repository URL.
