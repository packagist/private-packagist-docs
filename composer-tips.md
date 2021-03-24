# Composer Tips

### Composer update performance

To get a better understanding of what is happening during a Composer command like `composer update` or `composer install` you can add the option `-vvv` which enables very verbose debug output. The option `--profile` can help identify in which steps exactly Composer takes a lot of time or uses too much memory.

##### Resolving dependencies

The snippet below shows the part of a composer update output which decides which packages should be installed. In cases where this takes a long time composer usually has to analyze a lot of packages.
```
Resolving dependencies through SAT
Looking at all rules.
Something's changed, looking at all rules again (pass #96)
Dependency resolution completed in 129.785 seconds
Analyzed 37751 packages to resolve dependencies
Analyzed 1518987 rules to resolve dependencies
```
The performance of `composer update` can be improved by reducing the amount of package versions Composer has to consider. This can be achieved by setting more restrictive version constraints in your composer.json. For instance if you require a package `"acme/useful-package": "^1.0"` and that package in the meantime had several new releases then adjusting the version constraint, to e.g. `"^1.2.3"` can make a big difference because it immediately excludes all `"1.0.*"` and `"1.1.*"` versions.
