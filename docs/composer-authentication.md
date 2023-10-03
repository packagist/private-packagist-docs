# Composer Authentication
## 

Running Composer commands against Private Packagist always requires authentication.

## Different types of tokens
Three different types of authentication tokens can be used to access Private Packagist.

### User access token
Every user has their own token that they can access either on the profile page or on the overview page of their organizations.
The token grants the user access to all organizations and subrepositories they are a member of.

### Organization authentication tokens
Admins and owners of an organization can create additional tokens on the organization settings page.
Organization authentication tokens are ideal for automated systems like your CI environment or deployments.

If your organization uses subrepository then you can also create additional tokens in subrepositories.
Tokens created in the organization settings do not grant you access to any of your subrepositories and tokens created in
a subrepository only grant you access to the subrepository the token was created in.

#### Read-only vs update tokens
There are two kinds of organization authentication tokens, read-only and update tokens.
Read-only tokens aren't suitable to run `composer update` as they do not allow automatic mirroring for new packages.
If you need to run `composer update` then either use a token with update access or your personal access token.
Please note that you will be charged for authentication tokens with update access as if they were user accounts.

### Private Packagist for Vendors customer tokens
Every Private Packagist for Vendors customer receives their own authentication token.
The token can only be used to install packages for the matching customer URL.

Please note that neither user tokens nor organization tokens can be used to install packages from a customer URL.

## Token format
An authentication token consists of three parts: a prefix, a 60 hexadecimal character long random part, and an eight hexadecimal character long checksum.

There are currently three different prefixes:
* `packagist_ort_`: Organization tokens with read-only access
* `packagist_out_`: Organization tokens with update access
* `packagist_uut_`: User tokens with update access

This format doesn't apply to authentication tokens generated for Private Packagist for Vendors customers and older tokens that haven't been regenerated recently.
These tokens only consist of the 60 hexadecimal character random part.

### How to calculate the checksum
To calculate the checksum of a token, calculate the CRC32 checksum using the prefix and random part, convert the number to hexadecimal
and asserting that the checksum is exactly eight characters long.

For example using PHP, you can use `substr(str_pad(dechex(crc32($prefix . $random)), 8, '0', STR_PAD_LEFT), 0, 8)`
