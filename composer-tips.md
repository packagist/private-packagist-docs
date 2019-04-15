# Composer Tips

### Composer update performance

To get a better understanding of what is happening during a `composer update` the command should be run with `-vvv` which enabled a more verbose output.

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
One simple way to improve the performance of you `composer update` is by reducing the amount of package versions which are being considered by composer. This can be achieved by setting more restrictive version constraints in your composer.json. For instance if in your composer.json you require a package "acme/useful-package:^1.0" and that package in the meantime had several new releases then adjusting the version constraint can make a big difference.
