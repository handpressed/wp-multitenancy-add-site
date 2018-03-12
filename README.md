# WordPress Multitenancy Add Site

Adds WordPress sites to [WP Multitenancy Boilerplate](https://github.com/handpressed/wp-multitenancy-boilerplate).

- [Features](#features)
- [Requirements](#requirements)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
	- [Themes](#themes)
	- [Plugins](#plugins)
	- [Constants](#constants)
- [Directory structure](#directory-structure)
- [Adding sites](#adding-sites)
- [See also](#see-also)
- [Credit](#credit)

## Features

- Improved directory structure
- Easy WordPress configuration with constants and environment files
- Environment variables with [PHP dotenv](https://github.com/vlucas/phpdotenv)
- Enhanced security (separated web root and secure passwords with [roots/wp-password-bcrypt](https://github.com/roots/wp-password-bcrypt))
- WordPress [multitenancy](https://en.wikipedia.org/wiki/Multitenancy) (a single instance of WordPress core, themes and plugins serving multiple sites)

## Requirements

- PHP 5.6+
- Composer
- WP Multitenancy Boilerplate

## Prerequisites

A WordPress instance installed with [WP Multitenancy Boilerplate](https://github.com/handpressed/wp-multitenancy-boilerplate).

## Installation

```bash
$ composer create-project handpressed/wp-multitenancy-add-site {directory}
```

Replace `{directory}` with the name of your new project, e.g. its domain name.

Composer will symlink the existing WordPress instance added with WP Multitenancy Boilerplate in `/var/opt/wp` to `{directory}/web/wp`.

Composer will also symlink `/var/opt/wp/wp-content/themes` to `web/app/themes`, `/var/opt/wp/wp-content/plugins` to `web/app/plugins` and `/var/opt/wp/wp-content/mu-plugins` to `web/app/mu-plugins`.

## Configuration

Open the `{directory}/conf/.env` file and add your new site's domain name (`WP_DOMAIN`) and database credentials (`DB_NAME`, `DB_USER`, `DB_PASSWORD`). You can also define the database `$table_prefix` (default is `wp_`) if required.

Set your site's vhost document root to `/path/to/{directory}/web`.

### Themes

Add themes in `{directory}/web/app/themes` as you would for a normal WordPress install. You can use the WordPress admin to update them.

### Plugins

[WordPress Packagist](https://wpackagist.org) is already registered in the `composer.json` file so any plugins from the [WordPress Plugin Directory](https://wordpress.org/plugins/) can easily be required.

To add a plugin, use `composer require <namespace>/<packagename>` from the command-line. If it's from WordPress Packagist then the namespace is always `wpackagist-plugin`, e.g.:

```bash
$ composer require wpackagist-plugin/jetpack:*
```

Whenever you add a new plugin or update WordPress core, run `composer update` to install your new packages.

Themes and plugins are installed in the symlinked `themes` and `plugins` directories in `/var/opt/wp/wp-content` and will be available to all multitenancy sites.

Note: Some plugins may make modifications to the core `wp-config.php` file. Any modifications to `wp-config.php` that are needed by an individual site should be moved to the site's `{directory}/config/wp-constants.php` file.

### Constants

Put custom core, theme and plugin constants in `{directory}/conf/wp-constants.php`.

## Directory structure

    ├── composer.json             → Manage versions of WordPress, plugins and dependencies
    ├── conf                      → WordPress configuration files
    │   ├── .env       	      → WordPress environment variables (DB_NAME, DB_USER, DB_PASSWORD required)
    │   ├── wp-constants.php      → Custom core, theme and plugin constants
    │   ├── wp-env-config.php     → Primary WordPress config file (wp-config.php equivalent)
    │   └── wp-salts.php          → Authentication unique keys and salts (auto generated)
    ├── vendor                    → Composer packages (never edit)
    └── web                       → Web root (vhost document root)
        ├── app                   → wp-content equivalent
        │   ├── mu-plugins        ↔ Must-use plugins symlinked to /var/opt/wp/wp-content/mu-plugins
        │   ├── plugins           ↔ Plugins Must-use plugins symlinked to /var/opt/wp/wp-content/plugins
        │   ├── themes            ↔ Themes Must-use plugins symlinked to /var/opt/wp/wp-content/themes
        │   └── uploads           → Uploads
        ├── index.php             → Loads the WordPress environment and template (never edit)
        └── wp                    ↔ WordPress core symlinked to /var/opt/wp (never edit)
	    	└── wp-config.php     → Required by WordPress - loads conf/wp-env-config.php (never edit)

`↔` denotes a symlink.

## See also

[WordPress Dotenv Boilerplate](https://github.com/handpressed/wp-env-boilerplate)

## Credit

Inspired by [roots/bedrock](https://github.com/roots/bedrock) and [wpscholar/wp-skeleton](https://github.com/wpscholar/wp-skeleton).