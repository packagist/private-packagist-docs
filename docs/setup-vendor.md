# Getting started with Private Packagist vendors on a Composer project

You can start using Private Packagist from a vendor in either a new or an existing Composer project.

### composer init
You can initialize a new Composer project using the Private Packagist repository from the vendor by following the required steps:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer init --name=acme/website --repository=https://acme-company.repo.packagist.com/customer1/` . You can find the customer repository URL on the customer Composer information page. 

![Vendor URL](/Resources/public/img/docs/vendor-url-20201020.png)

### composer create-project

If you would like to initialize a project using a template package named `acme/website` from your vendor, then these steps are required:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer create-project acme/website --add-repository --repository=https://acme-company.repo.packagist.com/customer1/` You can find the customer repository URL on the customer Composer information page. 

Once the command is finished, the created composer.json file will contain the added repositories:

```
"repositories": [
    {"type": "composer", "url": "https://acme-company.repo.packagist.com/customer1/"}
]
```

*Important Note:* While using the `add-repository` flag and in case a composer.lock file is present, the lock file will be deleted and an update will be run instead of install.
