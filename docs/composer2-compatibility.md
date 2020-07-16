# Composer 2.0 Compatibility

Private Packagist is fully compatible with both Composer versions 1 and 2, you do not need to make any changes to your Private Packagist configuration when upgrading to Composer 2.0.

## Upgrading
In order to upgrade to a current preview release of Composer 2.0 you need to run the following command:

```
composer.phar self-update --preview
```

If you want to switch back to version 1 you can either use `composer self-update --rollback` right afterwards or `composer self-update --1` at any any later point.

## Improvements

Composer 2.0 has significantly improved performance and memory usage. It ships with parallelized downloads of metadata and distribution files and makes use of HTTP/2 features for more efficient server communication.

Private Packagist Composer repositories fully support all new Composer 2 network protocol improvements, including HTTP/2, an improved compression algorithm to reduce the amount of required bandwidth and new API endpoints to provide better error messages.

## Resources

- Composer 2.0 Upgrade Guide https://github.com/composer/composer/blob/master/UPGRADE-2.0.md
- Composer Changelog https://github.com/composer/composer/blob/master/CHANGELOG.md
- Composer 2 Development Update from April 24, 2020 https://blog.packagist.com/composer-2-development-update/

