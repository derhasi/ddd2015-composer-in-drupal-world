<!-- .slide: class="ddd-intro" data-background="slides/images/ddd-background.jpg" -->

# Composer<br> in Drupal World
## Johannes Haseitl - @derhasi

===

## Me

* Johannes Haseitl
* [@derhasi](https://twitter.com/derhasi) everywhere
* Maintainer of [Master](https://www.drupal.org/project/master), [Search API Override](https://www.drupal.org/project/search_api_override), ...
* CEO of [undpaul GmbH](http://www.undpaul.de/)

![undpaul logo](slides/images/undpaul.png)

Note: derhasi everywhery in the web

===

# What is Composer?

Note:
- Anyone used composer?

---

![Composer logo](slides/images/logo-composer-transparent.png)

> Dependency Manager for PHP

[https://getcomposer.org](https://getcomposer.org/)

Note:
- basically similar to package managers like bower, npm, ...
- find instructions on getcomposer.org for installation and general usage

---

## Declaring dependencies

Add a `composer.json` to the root:

```json
{
  "name": "derhasi/toggl2redmine",
  "description": "Command line tool to sync toggl time entries to redmine",
  "require": {
    "php": ">=5.4.0",
    "ajt/guzzle-toggl": ">=0.9",
    "kbsali/redmine-api": "1.5.*",
    "symfony/console": "2.6.*",
    "symfony/config": "2.6.*",
    "symfony/yaml": "2.6.*"
  },
  "bin": [
    "toggl2redmine"
  ],
  "autoload": {
    "psr-0": {
      "": "src/"
    }
  },
  "license": "GPLv3",
  "authors": [
    {
      "name": "Johannes Haseitl",
      "email": "johannes@undpaul.de"
    }
  ]
}
```

[getcomposer.org/doc/04-schema.md](https://getcomposer.org/doc/04-schema.md)

Note:
- add a composer.json to the root of your project
- there you define everything => foremost the dependencies

---

## Install project

- `composer install`
- Fetches information from [Packagist.org](https://packagist.org/)
- Downloads all packages to the `vendor` directory. 

---

<video data-autoplay class="stretch" src="slides/videos/composer-install.mp4"></video>

Note:
- fetches from cache, when it was already installed locally

---

## composer.lock

- Keeps track of exact version/commit for every package installed
- Installs exact version/commit on `composer install`
- Improves performance on `composer install`

Note:
- exact dependencies are tracked
- if composer.lock is given, installs exact versions when reinitializing
- that makes it faster

---

## Packagist

<video data-autoplay class="stretch" src="slides/videos/packagist.mp4"></video>

Note:
- Main & default source for package information
- you can add your own projects to it

---

## A little bit more

- Dependency manager <!-- .element: class="fragment" -->
- <!-- .element: class="fragment" --> [Autoloader](https://getcomposer.org/doc/04-schema.md#autoload) for PHP dependencies 
  <br>(supports _PSR-4_, _PSR-0_, _classmap_ and _files_)
  <br>at `vendor/autoload.php`
- Binary shortcuts <!-- .element: class="fragment" -->
  (`vendor/bin/...`)
- <!-- .element: class="fragment" --> [Script events and custom commands](https://getcomposer.org/doc/articles/scripts.md)
  ```
  "scripts": {
    "post-install-cmd": ...,
    "dependency-cleanup": ...
  }
  ```
- <!-- .element: class="fragment" --> [Composer plugins](https://getcomposer.org/doc/articles/plugins.md)

Note:
- DM: the main feature
- builds Autoloader
- binary, like toggl2redmine - global commands
- script events and composer plugins: custom PHP scripts or shell commands
  like create or modify files when project is installed
- Plugins: installable, reusable and ready to go

---

# Why use Composer?

- "Getting off the island!" <!-- .element: class="fragment" -->
- Architecture allows custom workflows <!-- .element: class="fragment" -->
- Drupal 8 already uses it <!-- .element: class="fragment" -->

Note:
- Used by a lot of PHP frameworks, like symfony, laravel, silex, Yii
- and more components
- Architecture: scripts, plugins
- D8 uses it -> we have a look at it

===

# Current use<br> in Drupal 8

![Drupal 8](slides/images/d8.png)

---

## Dependencies

`core/composer.json`:

```
{
  "name": "drupal/core",
  "description": "Drupal is an open source content management platform powering millions of websites and applications.",
  "type": "drupal-core",
  "license": "GPL-2.0+",
  "require": {
    "php": ">=5.4.5",
    "sdboyer/gliph": "0.1.*",
    "symfony/class-loader": "2.6.*",
    "symfony/css-selector": "2.6.*",
    "symfony/dependency-injection": "2.6.*",
    "symfony/event-dispatcher": "2.6.*",
    "symfony/http-foundation": "2.6.*",
    "symfony/http-kernel": "2.6.*",
    "symfony/routing": "2.6.*",
    "symfony/serializer": "2.6.*",
    "symfony/validator": "2.6.*",
    "symfony/process": "2.6.*",
    "symfony/yaml": "2.6.*",
    "twig/twig": "1.16.*",
    "doctrine/common": "~2.4.2",
    "doctrine/annotations": "1.2.*",
    "guzzlehttp/guzzle": "~5.0",
    "symfony-cmf/routing": "1.3.*",
    "easyrdf/easyrdf": "0.9.*",
    "phpunit/phpunit": "4.4.*",
    "zendframework/zend-feed": "2.4.*",
    "mikey179/vfsStream": "1.*",
    "stack/builder": "1.0.*",
    "egulias/email-validator": "1.2.*",
    "behat/mink": "~1.6",
    "behat/mink-goutte-driver": "~1.1",
    "fabpot/goutte": "^2.0.3",
    "masterminds/html5": "~2.1"
  },
  "autoload": {
    "psr-4": {
      "Drupal\\Core\\": "lib/Drupal/Core",
      "Drupal\\Component\\": "lib/Drupal/Component",
      "Drupal\\Driver\\": "../drivers/lib/Drupal/Driver"
    },
    "files": [
      "lib/Drupal.php"
    ],
    "classmap": [
      "lib/Drupal/Component/Utility/Timer.php",
      "lib/Drupal/Component/Utility/Unicode.php",
      "lib/Drupal/Core/Database/Database.php",
      "lib/Drupal/Core/DrupalKernel.php",
      "lib/Drupal/Core/DrupalKernelInterface.php",
      "lib/Drupal/Core/Site/Settings.php",
      "vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/Request.php",
      "vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/ParameterBag.php",
      "vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/FileBag.php",
      "vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/ServerBag.php",
      "vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/HeaderBag.php",
      "vendor/symfony/http-kernel/Symfony/Component/HttpKernel/HttpKernel.php",
      "vendor/symfony/http-kernel/Symfony/Component/HttpKernel/HttpKernelInterface.php",
      "vendor/symfony/http-kernel/Symfony/Component/HttpKernel/TerminableInterface.php"
    ]
  },
  "config": {
    "preferred-install": "dist",
    "autoloader-suffix": "Drupal8"
  }
}
```

Note:
- symfony components, guzzle, twig

---

## Autoloader

`core/autoload.php`

```
<?php

/**
 * @file
 * ...
 */

return require __DIR__ . '/core/vendor/autoload.php';

```

Note:
- uses the composer autoloader

===

# Composer in your Drupal project

---

## Setting up core

Set up a composer.json with:

```json
{
  "name": "yourname/yourprojectname",
  "type": "project",
  "require": {
    "composer/installers": "^1.0.20",
    "drupal/core": "8.0.*"
  },
  "minimum-stability": "dev",
  "prefer-stable": true
}
```

- <!-- .element: class="fragment" --> [`drupal/core` subtree split](https://packagist.org/packages/drupal/core) by tstoeckler
- <!-- .element: class="fragment" --> Component dependencies put in `./vendor`
- <!-- .element: class="fragment" --> [Composer Installers plugin](https://github.com/composer/installers) puts Core in `./core` by default.
- <!-- .element: class="fragment" --> (!) Manually add: index.php, updated autoload.php, .htaccess, ...

Note:
- name and type are not mandatory
- components like symfony, guzzle put in `./vendor`
- All Drupal functionality packed in /core
- More flexibility for "Front Controller"
- Autoloader will include all additional packages added to composer.json

---

## Adding modules / projects

`composer require drupal/micro:^8.2.0`

<video data-autoplay class="stretch" src="slides/videos/composer-require.mp4"></video>

- type `drupal-module`
- placed in `modules/{name}` by _composer-installers_
- [Version constraints](https://getcomposer.org/doc/01-basic-usage.md#package-versions)
  `^1.2.0` vs `>=1.2.0`

Note:
- Drupal module already on packagist.org
- Needs to have type drupal-module
- Composer Installers takes care

---

## Adding custom repositories<br> and packages

```
"repositories": [
  {
    "type": "vcs",
    "url": "ssh://gitolite@yourgitserver.com/up_p_download.git"
  },
  {
    "type": "package",
    "package": {
      "type": "drupal-module",
      "name": "drupal-sandbox/disable_defaults",
      "version": "dev-7.x-1.x",
      "source": {
        "url": "http://git.drupal.org/sandbox/derhasi/2004516.git",
        "type": "git",
        "reference": "7.x-1.x"
      }
    }
  },
  {
    "type": "package",
    "package": {
      "type": "component",
      "name": "leaflet/leaflet",
      "version": "0.7.3",
      "dist": {
        "url": "http://leaflet-cdn.s3.amazonaws.com/build/leaflet-0.7.3.zip",
        "type": "zip"
      }
    }
  }
]
```

- [`repositories` section in composer.json](https://getcomposer.org/doc/04-schema.md#repositories)
- Use [package server (e.g. satis)](https://getcomposer.org/doc/05-repositories.md#hosting-your-own) instead when possible

Note:
- different repository types: vcs, package, composer
- create new packages
- override existing => order
- linking to a repository directly is slower
- Setup Satis creates index like packagist.org

---

## Drupal Packagist

![Drupal Packagist icon](slides/images/drupal-packagist.svg)

Packagist server with package information for every project on drupal.org:
http://packagist.drupal-composer.org/

Note:
- solve adding missing drupal projects
- you do not need to add your project to packagist.org
- Drupal Packagist automatically grabs necessary information

---

### Implementation

```
  "repositories": [{
    "type": "composer",
    "url": "http://packagist.drupal-composer.org"
  }],
```

Issues on github: [drupal-composer/drupal-packagist](https://github.com/drupal-composer/drupal-packagist)
and [drupal-composer/drupal-parse-composer)](https://github.com/drupal-composer/drupal-parse-composer).

---

### Examples

```
composer require drupal/drupal:7.*
composer require drupal/ctools:7.*
composer require drupal/radix:7.*
composer require drupal/drush_language:7.*
```

Note:
- with Drupal Packagist you can ...
- even Drupal 7
- and all Drupal Modules, Themes, Drush extensions

---

## Submodules and metapackages

- Prevent packages from being installed: `replace`
- [Drupal packagist will likely provide metapackages](https://github.com/drupal-composer/drupal-packagist/issues/15)

```
"replace": {
  "drupal/action": "self.version",
  "drupal/aggregator": "self.version",
  "drupal/ban": "self.version",
  "drupal/bartik": "self.version",
  "drupal/basic_auth": "self.version",
  "drupal/block": "self.version",
  "drupal/block_content": "self.version",
  "drupal/book": "self.version",
  "drupal/breakpoint": "self.version",
  "drupal/ckeditor": "self.version",
  "drupal/classy": "self.version",
  "drupal/color": "self.version",
  "drupal/comment": "self.version",
  "drupal/config": "self.version",
  "drupal/config_translation": "self.version",
  "drupal/contact": "self.version",
  "drupal/content_translation": "self.version",
  "drupal/contextual": "self.version",
  "drupal/datetime": "self.version",
  "drupal/dblog": "self.version",
  "drupal/editor": "self.version",
  "drupal/entity_reference": "self.version",
  "drupal/field": "self.version",
  "drupal/field_ui": "self.version",
  "drupal/file": "self.version",
  "drupal/filter": "self.version",
  "drupal/forum": "self.version",
  "drupal/hal": "self.version",
  "drupal/help": "self.version",
  "drupal/history": "self.version",
  "drupal/image": "self.version",
  "drupal/language": "self.version",
  "drupal/link": "self.version",
  "drupal/locale": "self.version",
  "drupal/menu_link_content": "self.version",
  "drupal/menu_ui": "self.version",
  "drupal/migrate": "self.version",
  "drupal/migrate_drupal": "self.version",
  "drupal/minimal": "self.version",
  "drupal/node": "self.version",
  "drupal/options": "self.version",
  "drupal/path": "self.version",
  "drupal/phptemplate": "self.version",
  "drupal/quickedit": "self.version",
  "drupal/rdf": "self.version",
  "drupal/responsive_image": "self.version",
  "drupal/rest": "self.version",
  "drupal/search": "self.version",
  "drupal/serialization": "self.version",
  "drupal/seven": "self.version",
  "drupal/shortcut": "self.version",
  "drupal/standard": "self.version",
  "drupal/stark": "self.version",
  "drupal/statistics": "self.version",
  "drupal/syslog": "self.version",
  "drupal/system": "self.version",
  "drupal/taxonomy": "self.version",
  "drupal/telephone": "self.version",
  "drupal/text": "self.version",
  "drupal/toolbar": "self.version",
  "drupal/tour": "self.version",
  "drupal/tracker": "self.version",
  "drupal/twig": "self.version",
  "drupal/update": "self.version",
  "drupal/user": "self.version",
  "drupal/views": "self.version",
  "drupal/views_ui": "self.version"
},
```

Note:
- Submodules might be referenced as dependencies
- Resolving with using "replace"
- Drupal Packagist might provide metapackages to make them referencable

---

## Naming convention

Packages in Drupal context have to follow the 
[Drupal composer package naming convention (WIP)](https://www.drupal.org/node/2401519).

Current state:

```
drupal/drupal
drupal/ctools
drupal/views
drupal/datetime
drupal/core-datetime
```

Note:
- Drupal Packagist will follow this convention.

---

## Patches

- [Composer patches plugin (Netresearch)](https://packagist.org/packages/netresearch/composer-patches-plugin) or 
  [composer-patcher by jp-stacey/webflo](https://github.com/webflo/composer-patcher)
- Work needed for better experience
- Alternative: create (temporary) forks

```
 {
    "type": "package",
    "package": {
      "name": "yourname/yourproject-patches",
      "version": "1.0.10",
      "type": "patches",
      "require": {
        "netresearch/composer-patches-plugin": "~1.0"
      },
      "extra": {
        "patches": {
          "drupal/file_entity": {
            "dev-7.x-2.x": [
              {
                "title": "Panelizer settings aren't saved (Issue: https://www.drupal.org/node/2450235)",
                "url": "https://www.drupal.org/files/issues/panelizer_settings-2450235-1.patch"
              }
            ]
          },
          "drupal/panels": {
            "7.3.5": [
              {
                "title": "panels_mini_uninstall() fails when block module is not installed (Issue: https://www.drupal.org/node/2441097)",
                "url": "https://www.drupal.org/files/issues/panels_mini_uninstall-2441097-1.patch"
              }
            ]
          },
          "drupal/drush_language": {
            "dev-7.x-1.x": [
              {
                "title": "Support for filter and sort in export (Issue: https://www.drupal.org/node/2463107)",
                "url": "https://www.drupal.org/files/issues/support_for_filter_and_sort-2463107-1.patch"
              }
            ]
          }
        }
      }
    }
  }
```

Note:
- Patches: Drupal Community used to workflow
- Composer plugin
- forks: multiple patches, rebase on HEAD
- Add this package as dependency

---

## Frontend libraries

- some Components on Packagist (see https://github.com/components/components)
- [composer-asset-plugins](https://packagist.org/packages/fxp/composer-asset-plugin) fetches info from bower and npm
- Alternative: adapt [bower](http://bower.io/) or [npm](https://www.npmjs.com/) natively

Note:
- Problem: multiple versions of same frontend library, e.g. for different themes

---

## Example for D7

```
{
  "name": "undpaul/some-project",
  "description": "Some drupal project",
  "authors": [
    {
      "name": "Johannes Haseitl",
      "email": "johannes@undpaul.de"
    }
  ],
  "repositories": [
    {
      "type": "composer",
      "url": "http://packagist.drupal-composer.org"
    },
    {
      "type": "vcs",
      "url": "https://github.com/derhasi/slick.git"
    },
    {
      "type": "vcs",
      "url": "https://github.com/lucastockmann/imagesloaded.git"
    },
    {
      "type": "vcs",
      "url": "ssh://gitolite@anygitserver.com/up_p_download.git"
    },
    {
      "type": "package",
      "package": {
        "type": "drupal-module",
        "name": "drupal-sandbox/disable_defaults",
        "version": "dev-7.x-1.x",
        "source": {
          "url": "http://git.drupal.org/sandbox/derhasi/2004516.git",
          "type": "git",
          "reference": "7.x-1.x"
        }
      }
    },
    {
      "type": "package",
      "package": {
        "type": "component",
        "name": "leaflet/leaflet",
        "version": "0.7.3",
        "dist": {
          "url": "http://leaflet-cdn.s3.amazonaws.com/build/leaflet-0.7.3.zip",
          "type": "zip"
        }
      }
    },
    {
      "type": "package",
      "package": {
        "name": "undpaul/project-patches",
        "version": "1.0.10",
        "type": "patches",
        "require": {
          "netresearch/composer-patches-plugin": "~1.0"
        },
        "extra": {
          "patches": {
            "drupal/file_entity": {
              "dev-7.x-2.x": [
                {
                  "title": "Panelizer settings aren't saved (Issue: https://www.drupal.org/node/2450235)",
                  "url": "https://www.drupal.org/files/issues/panelizer_settings-2450235-1.patch"
                }
              ]
            }
          }
        }
      }
    }
  ],
  "minimum-stability": "dev",
  "prefer-stable": true,
  "require": {
    "davidbarratt/custom-installer": "dev-master",
    "derhasi/composer-preserve-paths": "0.1.*",
    "drupal/drupal": "7.*",
    "drupal/ctools": "7.*",
    "drupal/devel": "7.*",
    "drupal/diff": "7.*",
    "drupal/entity": "7.*",
    "drupal/features": "7.*",
    "drupal/globalredirect": "7.*",
    "drupal/panels": "7.*",
    "drupal/panels_everywhere": "7.*",
    "drupal/paragraphs": ">7.1.0-beta6",
    "drupal/pathauto": "7.*",
    "undpaul/up_panels": "7.*",
    "lcdsantos/jQuery-Selectric": "1.8.7",
    "remybach/jQuery.superLabels": "2.0.3",
    "derhasi/staging": "1.*",
    "drupal/l10n_update": "7.*",
    "drupal/stage_file_proxy": "7.*",
    "drupal/textformatter": ">7.1.4",
    "drupal/role_export": "7.*",
    "drupal/role_delegation": "7.*",
    "drupal/navbar": "7.*",
    "components/modernizr": ">=2.6.2",
    "components/underscore": ">=1.5",
    "components/backbone": ">=1.0",
    "drupal/crumbs": "7.*",
    "drupal/neutral_paths": "7.*",
    "drupal/date": "7.*",
    "drupal/piwik": "7.*",
    "jquery-textfill/jquery-textfill": "0.*",
    "drupal/metatag": ">7.1.4",
    "undpaul/project-patches": "*",
    " ... and a lot more ...": "...."
  },
  "require-dev": {
    "drupal/drush_language": "dev-7.x-1.x",
    "drupal/registry_rebuild": "7.*",
    "drupal/master": "7.3.x-dev",
    "undpaul/shellwrapper": "dev-master"
  },
  "replace": {
    "drush/drush": "*",
    "bower-asset/jquery": "*",
    "components/jquery": "*",
    "robloach/component-installer": "*"
  },
  "autoload": {
    "psr-4": {
      "undpaul\\Composer\\": "src"
    }
  },
  "scripts": {
    "post-install-cmd": [
      "undpaul\\Composer\\ProjectScripts::removeFiles",
      "undpaul\\Composer\\ProjectScripts::localSettings"
    ],
    "post-update-cmd": "undpaul\\Composer\\ProjectScripts::removeFiles",
    "dependency-cleanup": [
      "rm -rf vendor",
      "rm -rf docroot/sites/all/modules/drupal* docroot/sites/all/modules/undpaul",
      "rm -rf docroot/sites/all/themes/drupal* docroot/sites/all/themes/undpaul",
      "rm -rf docroot/sites/all/libraries",
      "rm -rf docroot/misc docroot/includes docroot/modules docroot/themes docroot/profiles docroot/scripts",
      "rm -rf docroot/*.php docroot/*.txt docroot/web.config docroot/sites/*.php docroot/sites/*.txt",
      "rm -rf docroot/sites/all/drush/*/",
      "echo \"(!) Now you can run 'composer install' to get the latest dependencies.\""
    ]
  },
  "config": {
    "vendor-dir": "vendor"
  },
  "extra": {
    "custom-installer": {
      "drupal-module": "docroot/sites/all/modules/{$vendor}/{$name}/",
      "drupal-theme": "docroot/sites/all/themes/{$vendor}/{$name}/",
      "drupal-library": "docroot/sites/all/libraries/{$name}/",
      "drupal-drush": "docroot/sites/all/drush/{$name}/",
      "drupal-profile": "docroot/profiles/{$name}/",
      "drupal-core": "docroot/",
      "component": "docroot/sites/all/libraries/{$name}/",
      "library": "docroot/sites/all/libraries/{$name}/"
    },
    "preserve-paths": [
      "docroot/.htaccess.local",
      "docroot/.htaccess.stage",
      "docroot/.htaccess.live",
      "docroot/sites/all/modules",
      "docroot/sites/all/themes",
      "docroot/sites/all/libraries",
      "docroot/sites/all/drush",
      "docroot/sites/default/local.settings.php",
      "docroot/sites/default/shared.settings.php",
      "docroot/sites/default/settings.php",
      "docroot/sites/default/settings.local.php",
      "docroot/sites/default/settings.stage.php",
      "docroot/sites/default/settings.live.php",
      "docroot/sites/default/files"
    ]
  }
}
```

---

## Risk of overriding paths

- [Composer preserve paths plugin](https://packagist.org/packages/derhasi/composer-preserve-paths)
- [Drupal tangler](https://packagist.org/packages/drupal/tangler)

```
"require": {
  "composer/installers": "~1.0",
  "derhasi/composer-preserve-paths": "0.1.*",
  "drupal/drupal": "7.*"
},
"extra": {
  "preserve-paths": [
    "sites/all/modules/contrib",
    "sites/all/themes/contrib",
    "sites/all/libraries",
    "sites/all/drush",
    "sites/default/settings.php",
    "sites/default/files",
    ".htaccess",
    "robots.txt",
  ]
}
```

Note:
- Especially for D7
- derhasi/composer-preserve-paths
- drupal/tangler

---

## Project templates

```
composer create-project drupal-composer/drupal-project:8.* 
```

* Separate templates for D7 and D8
* https://github.com/drupal-composer/drupal-project
* You can create your own.

===

# Composer vs. Drush

---

## Can composer replace drush?

- <!-- .element: class="fragment" --> For most parts: *NO!*
- <!-- .element: class="fragment" --> But `drush dl`, `drush make`

Note: 
- Rule of thumb:
- Everything that "only" deals with source code or does not need the database

---

## Downloading Drupal projects

`composer require` can replace `drush dl`

- bound to a predefined directory pattern <br>(_composer installer plugins_)
- Dependencies are resolved automatically
- keeps track of exact versions with `composer.lock`
- <!-- .element: class="fragment" --> Do not use a mix of `drush dl` and `composer require`

---

## Drush make

- [Composer in relation to Drush make <br>(drupal.org/node/2471553)](https://www.drupal.org/node/2471553) 
- [Composer Generate <br>(drupal.org/project/composer_generate)](https://www.drupal.org/project/composer_generate)
  <br>like `drush make-generate`

===

# Community

---

## Thanks

- webflo
- winmillwill
- tstoeckler
- davidbarrat
- kasperg
- everyone I forgot

---

## Drupal projects

<ul>
  <li><a href="https://www.drupal.org/project/composer_manager">Composer Manager</a></li>
  <li><a href="https://www.drupal.org/project/composer">Composer</a></li>
  <li><a href="https://www.drupal.org/project/composer_generate">Composer Generate</a></li>
  <li><a href="https://www.drupal.org/project/composer_autoload">Composer Autoload</a></li>
  <li><a href="https://www.drupal.org/project/composer_autoloader">Composer Vendor Autoload</a></li>
</ul>

---

## Goals

- Make Drupal 8 core work with composer
- Easy adoption of Composer for Drupal
- Figuring out best practices:<br> [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project)

Note:
- ease of use: Drupal Packagist

---

## Tasks

- <!-- .element: class="fragment" --> **Test, test, test** Drupal Packagist
- <!-- .element: class="fragment" --> Update packagist.drupal-composer.org
- <!-- .element: class="fragment" --> Provide metapackages
- <!-- .element: class="fragment" --> More flexible installer: `composer/installers` vs `davidbarratt/custom-installer`
- <!-- .element: class="fragment" --> Missing `version=...` in `.info`-files => [Composify by bforchhammer(sandbox)](https://www.drupal.org/sandbox/bforchhammer/2375797)
- <!-- .element: class="fragment" --> Improve patch workflow
- <!-- .element: class="fragment" --> Handling different versions of same package
- <!-- .element: class="fragment" --> Handling frontend-libraries and assets
- <!-- .element: class="fragment" --> **a lot lot more**

---

## Meta document

### on groups.drupal.org/composer

https://groups.drupal.org/node/450258#Issues

---

## drupal-composer.org

[![Drupal Composer icon](slides/images/drupal-composer.png)](http://drupal-composer.org/)

Join us!

===

# Thank you!

===

# Q & A
