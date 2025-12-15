# Reading package metadata via API

To read the package metadata via API, use the Composer API on your Private Packagist repository.

This is useful when you need to programmatically access package metadata beyond what's available through the Private Packagist API
(for example to read custom configuration from the `extra` section of a package's composer.json). 

Read more about the API: https://packagist.org/apidoc#get-package-data


### Example implementation

The process works as follows:
1. Get the JSON from `https://repo.packagist.com/<org>/p2/<package>.json`
2. Authenticate using username `token` and an Authentication Token as password
3. Decode the JSON as nested arrays
4. Use [MetadataMinifier](https://github.com/composer/metadata-minifier) to expand the metadata for each package version: `$metadata = MetadataMinifier::expand($data['packages'][$package])`

```bash
# Make sure the composer/metadata-minifer package is installed:
composer require composer/metadata-minifier
```

```php
use Composer\MetadataMinifier\MetadataMinifier;

// Assuming your organization name is acme-company and the package name is acme/package
$organization = 'acme-company'; 
$package = 'acme/package'; 

// NOTE: You must use an authentication token for the composer API, not the Private Packagist API credentials.
// You can create a token in your Organization settings > Authentication Tokens
$token = 'packagist_ort_..........'; 

// The p2/$vendor/$package.json endpoint contains only tagged releases. If you want to fetch information about branches (i.e. dev versions) 
// you need to use p2/$vendor/$package~dev.json.
$apiUrl = sprintf('https://repo.packagist.com/%s/p2/%s.json', $organization, $package); 

$ch = curl_init($apiUrl); 
curl_setopt_array($ch, [ 
    CURLOPT_RETURNTRANSFER => true, 
    CURLOPT_HTTPAUTH => CURLAUTH_BASIC, 
    CURLOPT_USERPWD => 'token:' . $token
]); 

$response = curl_exec($ch); 
curl_close($ch); 

$data = json_decode($response, true); 
$metadata = MetadataMinifier::expand($data['packages'][$package]);
```


