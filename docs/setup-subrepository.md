# Private Packagist Subrepositories
##

Subrepositories are separate Composer repositories with their own URL, authentication tokens, and a separate list of packages.

By default, a subrepsitory will only have access to mirrored packages from packagist.org. You define which mirrored third-party
repositories, shared organization packages, and credentials can be used in a subrepository.

## When to use subrepositories?

Subrepositories are an optional feature that helps you separate multiple Composer projects. There are some use cases where 
we recommend using subrepositories but you can always start without subrepositories and add them later.

### Managing different client projects

When working on client projects you often might have packages that should only be available to a specific client e.g.
a design you built, or a third party package that you purchased. With subrepositories you can add those packages only 
to the client's subrepository and nobody else will be able to install them. This is particularly useful if your clients
run Composer commands to avoid them installing packages they should not have access to.

In case your clients require access to Private Packagist you can invite them as collaborators to the subrepository. This
way they won't have access to your organization but will only be able to see the subrepository they were invited to.

### Composer projects using different major versions of a framework

Several frameworks like Magento and Drupal use a different set of packages for each major framework version.
Drupal, for instance, uses a different mirrored third-party repository for each major version from where packages can be installed.
Each of these repositories might contain packages with the same name that only work in one specific major version.

This is where subrepositories can help. Start with one subrepository per Drupal version e.g.
one for Drupal 7 and in all your Composer projects that use Drupal 7 you set up that subrepository. This will make sure
that you never install a package for the wrong Drupal version in one of your Composer projects. 

## Setting up a subrepository in Private Packagist

Organizations owners and admins can create subrepositories on the subrepositories page via the "Create subrepositories" button.
On the settings page of an individual team you can also grant all members of a team permission to create subrepositories.

You can also create subrepositories using our API through the [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$subrepositoryName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');
$customer = $client->subrepositories()->create($subrepositoryName);
```

### Setting up mirrored third party repositories and credentials 

On the subrepository settings page you can configure the mirrored third-party repositories and credentials that can be used in
the subrepository.
Accessing the mirrored third-party repository settings page allows you to either add existing mirrors
from the organization or configure additional mirrored third-party repositories. The same applies for credentials on the credential page.

Your organization settings page allows you to see in which subrepositories a specific mirrored third-party repository
or credential is available. You can also set them up to be automatically added to every new subrepository that
you create to facilitate the setup of new subrepositories.

Mirrored third-party repositories can also be added to a subrepository via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$subrepositoryName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$mirroredRepositories = $client->mirroredRepositories()->all();
// Find the id of the mirrored third-party repository you are looking for
$mirroredRepositoryId = array_pop($mirroredRepositories)['id'];

$mirroredRepositoryData = [
  'id' => $mirroredRepositoryId,
  'mirroringBehavior' => 'add_on_use',
];

$client->subrepositories()->mirroredRepositories()->add($subrepositoryName, [$mirroredRepositoryData]);
```

### Adding packages

Existing packages can be added on the subrepository's packages page via "Add Package" -> "Organization Packages".
Select all necessary packages and add them to the subrepository. If you need to add additional packages to a subrepository
that do not yet exist in the organization you can add them using the "Add Package" dialog and the previously set up credentials.

Mirrored packages will automatically be added to your subrepository on use. We do not recommend manually adding them to your subrepository.

Packages can also be added via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$subrepositoryName = 'company-blog';
$url = 'https://github/acme/company-blog-design';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');
$client->subrepositories()->packages()->createVcsPackage($subrepositoryName, $url);
```

### Granting members access

Permissions in a subrepository are managed via team, the same way permissions are managed in your organization.
You can add teams to a subrepository and all members of that team will automatically be granted access to it.
Team membership changes e.g. through a synchronization will automatically be applied to subrepositories as well.

One notable difference in subrepositories is that all members automatically have access to all packages inside a subrepository.
You do not have to manually grant access to packages.

Teams can also be added to a subrepository via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$subrepositoryName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$teams = $client->teams()->all($subrepositoryName, $url);
// find id of team to be added
$teamId = array_pop($teams)['id'];

$team = [
  'id' => $teamId,
  'permission' => 'view',
];

$client->subrepositories()->addOrEditTeams($subrepositoryName, [$team]);
```

### Granting external collaborators access to a subrepository

Working on a Composer project with external developers might sometimes require that those developers get access to a subrepository
to make sure they can run composer install and update and help manage the project's dependencies. Adding them as subrepository
collaborators will only grant them access to selected subrepositories and not give them access to you your Private Packagist organization.

You can invite via email subrepository collaborators on the subrepository's team page via the "Manage Collaborators" button.


## Setting up CI / CD environments

We recommend that you create authentication tokens for all your CI / CD environments and do not use your personal authentication token for that purpose.

You can create authentication tokens on the settings page of the subrepository page under authentication tokens.
Please be aware that tokens created on the organization's settings page will only have access to the organization's Composer repository
and tokens created in a subrepository will only have access to that specific subrepository's Composer repository.

Authentication tokens can also be created via our API using our [API client](https://github.com/packagist/private-packagist-api-client) with the following code snippet:

```
<?php

require_once __DIR__ . '/vendor/autoload.php';

$subrepositoryName = 'company-blog';

$client = new \PrivatePackagist\ApiClient\Client();
$client->authenticate('api-token', 'api-secret');

$tokenData = [
  "description" => "Blog deploy token",
  "access" => "read"
];
$token = $client->subrepositories()->createToken($subrepositoryName, $tokenData);
```
