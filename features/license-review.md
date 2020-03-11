# Dependency License Review
## 

License Review helps you understand and manage legal challenges with your dependencies.

You can find your Dependency License Review in your organization's "Packages" tab. You will find a list of all licenses used in any of your packages. License Review allows you to inspect which versions of which dependency are available under which license. Links to details on the respective open-source licenses help you find legal information regarding the licenses.

![License Review](/Resources/public/img/docs/features/License-Review-20170110.png#no-spacing)
Dependency License Review for the phpBB organization on Private Packagist

## Open-Source Licenses
Open-source licenses come in a **lot of flavours** like weak and strong **copyleft** or **permissive** software licenses. In addition to the **great benefit** of being able to reuse code, open-source licenses present **some challenges for businesses**. With a complex project making use of open-source frameworks and libraries you will have to determine which open-source licenses allow you to incorporate which work into your own. This depends on the type of product you are building, whether you are providing a service or shipping code to customers, and the license you pick yourself.
            
The **[Open Source Initiative](https://opensource.org)** is a great resource to learn more about open-source licenses. They have implemented a review process for open-source licenses so it becomes easier to determine whether a given software license is an open-source license at all.

## SPDX: Standardized Open-Source License identifiers
The **[Software Package Data Exchange (SPDX)](https://spdx.org)** curates a list of license identifiers that enable automation around licenses in complex systems made up of large numbers of components. Composer makes use of this list with its composer/spdx-licenses library. Composer warns you if the license in your composer.json “license” key cannot be identified using this library. **If you maintain any open-source package, please review your composer.json and ensure that you are using a valid SPDX license identifier** to help your users manage their dependencies.

Based on the SPDX identifiers Private Packagist License Review provides a list of all open-source licenses used by packages in your package repository. You can browse packages by license, and see if the licenses for a package changed over time.

If you know of or find packages using Private Packagist License Review, which do not use an SPDX identifier, please get in touch with the maintainers or simply send them a pull-request. Often it’s simply a matter of slightly modifying the identifier. By the way, if your package is **dual licensed**, please specify an **array of licenses** in your composer.json instead of hardcoding the word “or” into the string to help automated systems understand your licensing.
            
## Private Packagist License Management Roadmap
This is merely the first tool to help you manage the licenses of your dependencies. We’re planning to expand on this functionality by allowing you to define a set of open-source licenses, to allow or reject for new packages, in order to prevent your developers from accidentally requiring packages with an incompatible license. Once we implement notifications, we’ll make you aware of new licenses that you should review when they are first added to your repository.
