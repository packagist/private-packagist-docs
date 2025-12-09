# Reading package metadata via API

To read the package metadata via API, use the Composer API on your Private Packagist repository.

Read more about the API: https://packagist.org/apidoc#get-package-data

### Example implementation

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


