> [!WARNING]
> **This repository is no longer maintained by our internal teams.**  
> The template is provided *as is* and will not receive updates, bug fixes, or new features.  
> You are welcome to contribute on it or fork the repository and modify it for your own use.
> To deploy this template on [Upsun](https://www.upsun.com), you can use the command [upsun project:convert](https://docs.upsun.com/administration/cli/reference.html#projectconvert)
> on this codebase to convert the existing `.platform.app.yaml` configuration file to the [Upsun Flex format](https://docs.upsun.com/create-apps/app-reference/single-runtime-image.html).

# TYPO3 for Platform.sh

This template builds the TYPO3 CMS for Platform.sh.  It comes pre-configured with MariaDB for storage and Redis for caching.  A command line installer will automatically initialize the site on first deploy.

TYPO3 is a Content Management System (CMS) built in PHP.

## Features

* PHP 7.4
* MariaDB 10.4
* Redis 5.0
* Automatic TLS certificates
* Composer-based build

## Post-install

1. The first time the site is deployed, the TYPO3 console will initialize the database and create an initial user.  It will not run again unless the `installer/.platform.installed` is removed.  (Do not remove that file unless you want the installer to run on the next deploy!)

2. The installer will create an administrator account with username/password `admin`/`password`.  **You need to change this password immediately. Not doing so is a security risk**.

3. Enable the `pixelant/pxa-lpeh` plugin.  It is already installed by default but must be enabled manually.  This plugin changes TYPO3's 403/404 page handling to avoid a self-request HTTP request that can cause a race condition and deadlocks in some situations. See the [TYPO3 documentation](https://docs.platform.sh/frameworks/typo3.html#avoiding-deadlock) for more information.

## Customizations

The following changes have been made relative to TYPO3 as it is downloaded from typo3.org.  If using this project as a reference for your own existing project, replicate the changes below to your project.

* The `.platform.app.yaml`, `.platform/services.yaml`, and `.platform/routes.yaml` files have been added.  These provide Platform.sh-specific configuration and are present in all projects on Platform.sh.  You may customize them as you see fit.
* An additional Composer library, [`platformsh/config-reader`](https://github.com/platformsh/config-reader-php), has been added.  It provides convenience wrappers for accessing the Platform.sh environment variables.
* A `sites/main/config.yaml` file is provided that reads the base URL from an environment variable.
* A default `public/typo3conf/AdditionalConfiguration.php` file is provided that loads a `PlatformshConfiguration.php` file if found.  That file maps Platform.sh environment variables to the database, Redis, and base URL environment configuration needed by TYPO3.  You may modify both of these files as needed.
* The [`pixelant/lpeh](https://extensions.typo3.org/extension/pxa_lpeh/) plugin is included via Composer, but must be enabled manually post-installation.
* Specifically, a relationship named `database` will automatically be wired to the TYPO3 primary database.  Additionally, if a relationship named `rediscache` is defined it will be used as the cache backend.  (It is included in this template.)

## References

* [TYPO3](https://typo3.org/)
* [PHP on Platform.sh](https://docs.platform.sh/languages/php.html)
