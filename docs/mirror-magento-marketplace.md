# Mirroring Magento Marketplace Packages
## 

Private Packagist supports mirroring Composer repositories which require authentication. This expands on the mirroring functionality we provide for Packagist.org and other open Composer repositories like Drupal’s or Wordpress’.

Mirrored packages show up in your Private Packagist search results, your [License Review](../features/license-review.d), and can be installed from your Private Packagist repository with Composer. Their distribution files are cached in your Private Packagist organization to make downloads redundant and faster.

To setup credentials for a repository, “Magento Marketplace” in our example, head to Settings &gt; Manage Credentials in your Private Packagist organization. Afterwards you can add a new mirrored third party repository under Settings as well. From then on packages can be manually or automatically mirrored based on your configuration.

## Magento Marketplace Walkthrough
Start by hitting “Manage Credentials” in your organizations settings. My organization is called “ACME Company”.

![Manage Credentials](/Resources/public/img/docs/features/magento/Magento-Credentials-20200723.png)

Next enter a description, I’m going with “Magento Marketplace Credentials”, and select “Magento Marketplace” as the authentication type. The domain “repo.magento.com” will be filled in automatically. Now we need the Magento Marketplace Public and Private Key, we’ll click on “My Access Keys” to get to the Magento Marketplace website.

![Create Credentials](/Resources/public/img/docs/features/magento/Magento-Credentials-Edit-20200723.png)

Make sure you’re on the Magento 2 Access Keys page, and then copy and paste the Private and Public keys into the respective fields on Private Packagist. Once you’re done, hit “Create” on the Private Packagist Credentials page.

![Magento Access Keys](/Resources/public/img/docs/features/magento/Magento-Access-Keys.png)

With the credentials set up we can now mirror a new Composer repository under Settings in the Private Packagist organization.

![Mirror Settings](/Resources/public/img/docs/features/magento/Magento-Settings-Mirror-20200723.png)

Pick a name for the repository, I picked “Magento Marketplace”, enter the URL “https://repo.magento.com” and pick the credentials we just set up for authentication with this repository. You can choose whether you would like packages to be mirrored automatically when someone tries to access them through composer update, or if you would like to add them yourself manually.

![Mirror Add](/Resources/public/img/docs/features/magento/Magento-Mirror-Add-20200723.png)

Under Packages we can now add a package “From Mirror”.

![Packages Add](/Resources/public/img/docs/features/magento/Magento-Packages-Add-2020723.png)

I’m going to enter just one package for the demo, but you can add as many as you like here. I’m going with “magento/module-catalog”. Make sure to select the “Magento Marketplace” Mirror repository underneath, and hit “Add”.

![Packages Add Proxied](/Resources/public/img/docs/features/magento/Magento-Packages-Add-2020723.png)

Private Packagist downloads the package metadata in the background and then notifies us that the package has been initialized, and it’s now accessible through composer update & composer install and shows up on package searches in Private Packagist!

![Packages View](/Resources/public/img/docs/features/magento/Magento-Packages-View-20200723.png)
