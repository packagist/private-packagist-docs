# Using Private Packagist with the vendor addon in a Composer project

To start a new project using Composer and Private Packagist with the [vendor](https://packagist.com/vendors) addon, you can use the following command:

1. Configure authentication on your machine. The command to set an authentication token is listed on the customer Composer information page.
2. Run `composer create-project acme/website --add-repository --repository=http://acme-company.repo.packagist.com/customer1/` You can find the customer repository URL on the customer Composer information page. 

*Important Note:* While using the `add-repository` flag and in case a composer.lock file is present, the lock file will be deleted and an update will be run instead of install.
