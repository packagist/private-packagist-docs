# Private Packagist Suborganizations
##

Suborganizations are additional Composer repositories in an organization with their own URL, authentication tokens, and a separate list of packages.

You can select which mirrored third-party repositories, shared organization packages, and credentials can be used in each suborganization and which users should have access to a suborganization. Synchronization of packages with GitHub, Bitbucket or GitLab can be set up for a suborganization separately from the organization's synchronizations.

## When to use suborganizations?

Suborganizations are an optional feature which helps you separate the packages and configuration for different Composer projects. The following are some of the use cases for which 
we recommend using suborganizations. You can also begin using Private Packagist without suborganizations and add them later.

### Agencies: Managing client projects

When working on client projects you may have packages which should only be available to a particular client e.g.
a design you built, or a third party package that you purchased. Suborganizations allow you to add these packages only 
to the client's Composer repository while other client projects will remain unable to install them. This is particularly useful if your clients run Composer commands themselves to prevent them from installing packages they should not have access to.

In case your clients require access to Private Packagist you can invite them as collaborators to the suborganization. They won't have access to your organization but will be able to work with the suborganization they were invited to.

### Composer projects using different major versions of a framework

Several frameworks like Magento and Drupal use a different set of packages for each major framework version.
Drupal, for instance, uses a different mirrored third-party repository for each major version from where packages can be installed.
Each of these repositories may contain packages with the same name that only work with a particular major framework version.

Suborganizations can help separate your packages for such projects. Start with one suborganization for each major Drupal version, e.g.
one for Drupal 7 and one for Drupal 8. Then change your composer.json in all Composer projects using Drupal 7 and 8 to load packages from the respective suborganization's URL. Now you will never accidentally install a package for the wrong Drupal version in one of your Composer projects.

## Setting up a suborganization in Private Packagist

Organization owners and admins can create suborganizations on the suborganizations page via the "Create suborganization" button.
On the settings page of an individual team you can grant all members of a team permission to create suborganizations.

Alternatively, you can create suborganizations using our API through the [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$suborganizationName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');
$customer = $client->suborganizations()->create($suborganizationName);
```

### Setting up mirrored third party repositories and credentials 

On the suborganization settings page you can configure the mirrored third-party repositories and credentials that can be used in
the suborganization.
Accessing the mirrored third-party repository settings page allows you to either add existing mirrors
from the organization or configure additional mirrored third-party repositories for the suborganization only. The same applies for credentials.

Your organization settings page allows you to see in which suborganizations a specific mirrored third-party repository
or credential is available. You can configure them to be automatically added to every new suborganization which
you create to facilitate the setup of new suborganizations.

Alternatively, mirrored third-party repositories can be added to a suborganization via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$suborganizationName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$mirroredRepositories = $client->mirroredRepositories()->all();
// Find the id of the mirrored third-party repository you are looking for
$mirroredRepositoryId = array_pop($mirroredRepositories)['id'];

$mirroredRepositoryData = [
  'id' => $mirroredRepositoryId,
  'mirroringBehavior' => 'add_on_use',
];

$client->suborganizations()->mirroredRepositories()->add($suborganizationName, [$mirroredRepositoryData]);
```

### Adding packages

Existing packages can be shared into the suborganization via "Add Package" -> "Organization Packages".
Select all necessary packages and add them to the suborganization, you can select to add their dependencies too. If you need to create new packages in a suborganization you can add them using the "Add Package" dialog and the previously set up credentials.

Mirrored packages will automatically be added to your suborganization on use via composer commands. We do not recommend manually adding them to your suborganization.

Alternatively, packages can be added via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$suborganizationName = 'company-blog';
$url = 'https://github/acme/company-blog-design';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');
$client->suborganizations()->packages()->createVcsPackage($suborganizationName, $url);
```

### Granting members access

Permissions in suborganizations are managed via teams, the same way permissions are managed in your organization.
You can add teams to a suborganization and all members of that team will be granted access to the suborganization.
Team membership changes e.g. through a synchronization will automatically be applied to suborganizations as well.

One notable difference in suborganizations is that all members automatically have access to all packages inside a suborganization.
You do not have to manually grant access to packages.

Alternatively, teams can be added to a suborganization via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$suborganizationName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$teams = $client->teams()->all($suborganizationName, $url);
// find id of team to be added
$teamId = array_pop($teams)['id'];

$team = [
  'id' => $teamId,
  'permission' => 'view',
];

$client->suborganizations()->addOrEditTeams($suborganizationName, [$team]);
```

### Granting external collaborators access to a suborganization

Working on a Composer project with external developers might sometimes require that those developers get access to a suborganization
to make sure they can run composer install and update and help manage the project's dependencies. Adding them as suborganization
collaborators will only grant them access to selected suborganizations and not give them access to you your Private Packagist organization.

You can invite via email suborganization collaborators on the suborganization's team page via the "Manage Collaborators" button.


## Setting up CI / CD environments

We recommend that you create authentication tokens for your CI / CD environments and do not use your personal authentication token on automated systems.

You can create authentication tokens on the settings page of the suborganization page under authentication tokens.
Please beware that tokens created on the organization's settings page will only have access to the organization's Composer repository
and tokens created in a suborganization will only have access to that specific suborganization's Composer repository.

Alternatively, authentication tokens can be created via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$suborganizationName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$tokenData = [
  "description" => "Blog deploy token",
  "access" => "read"
];
$token = $client->suborganizations()->createToken($suborganizationName, $tokenData);
```
