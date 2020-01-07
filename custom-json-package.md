# How to add a package via custom JSON
This document will show you how you can add a package via custom JSON .

## Adding a single package
Here is how the JSON structure look like:

```json
{
    "type": "package",
    "package": {
        "name": "smarty/smarty",
        "version": "3.1.7",
        "dist": {
            "url": "https://www.smarty.net/files/Smarty-3.1.7.zip",
            "type": "zip"
        },
        "source": {
            "url": "http://smarty-php.googlecode.com/svn/",
            "type": "svn",
            "reference": "tags/Smarty_3_1_7/distribution/"
        },
        "autoload": {
            "classmap": ["libs/"]
        }
    }
}
```
Important to note from the list above:
* dist: The packaged version of the package and it should be something that you provide (not hosted by Private Packagist.)
* source:  The source code repository such as git.

For a full detailed list about these attributes see [Composer docs](https://getcomposer.org/doc/05-repositories.md#package).

## Adding multiple versions of a package
In order to define multiple versions of a package, you can set an array with multiple versions:
```json
{
    "type": "package",
    "package": [
        {
            "name": "foo/bar",
            "version": "1.0.0",
            ...
        },
        {
            "name": "foo/bar",
            "version": "2.0.0",
            ...
        }
    ]
}
```