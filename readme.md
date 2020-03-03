These are general rulesets for PHP Code Sniffer.

- [Usage](#usage)
- [Laravel](#laravel)
- [WordPress](#wordpress)
  - [Initial Setup](#initial-setup)
  - [Project Setup](#project-setup)
    - [Minimum Versions](#minimum-versions)
    - [Translation](#translation)

# Usage

[PHP Code Sniffer](https://github.com/squizlabs/PHP_CodeSniffer) is a set of two PHP scripts; the main phpcs script that tokenizes PHP, JavaScript and CSS files to detect violations of a defined coding standard, and a second phpcbf script to automatically correct coding standard violations. PHP_CodeSniffer is an essential development tool that ensures your code remains clean and consistent.

This is the current `phpcs` plugin for Visual Studio Code: [https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs](https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs)

Recommended: in Visual Studio Code, set the default PHPCS standard to the plugin ruleset from this repo. Edit your `settings.json` and add this line: `"phpcs.standard": "/Users/<your name>/repositories/phpcs-rulesets/WordPress/plugin/phpcs.xml",`

# Laravel

See [Usage](#usage); there is no special usage required.

# WordPress

Read about the [WordPress Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/).

## Initial Setup

These WordPress and third-party coding standard repos must first be installed and added to PHPCS’ `installed_paths`:

```bash
# create directory to store rulests
mkdir -p ~/repositories
cd ~/repositories/

# clone rulesets
git clone git@github.com:PHPCompatibility/PHPCompatibility.git
git clone git@github.com:PHPCompatibility/PHPCompatibilityParagonie.git
git clone git@github.com:PHPCompatibility/PHPCompatibilityWP.git
git clone git@github.com:WordPress-Coding-Standards/WordPress-Coding-Standards.git

# configure PHPCS
phpcs --config-set installed_paths /Users/$(whoami)/repositories/PHPCompatibility/,/Users/$(whoami)/repositories/PHPCompatibilityParagonie/PHPCompatibilityParagonieRandomCompat/,/Users/$(whoami)/repositories/PHPCompatibilityParagonie/PHPCompatibilityParagonieSodiumCompat/,/Users/$(whoami)/repositories/PHPCompatibilityWP/PHPCompatibilityWP/,/Users/$(whoami)/repositories/WordPress-Coding-Standards/WordPress/,/Users/$(whoami)/repositories/WordPress-Coding-Standards/WordPress-Core/,/Users/$(whoami)/repositories/WordPress-Coding-Standards/WordPress-Docs/,/Users/$(whoami)/repositories/WordPress-Coding-Standards/WordPress-Extra/
```

## Project Setup

### Minimum Versions

By default, PHP 7.0 is set as the minimum supported version, and deprecated functions will be flagged.

By default, WordPress 4.8 is set as the minimum supported version, and deprecated functions will be flagged.

### Translation

Set the `WordPress.WP.I18n` rule’s `text_domain` property to your plugin/theme’s text domain. If you’re creating a child theme (Genesis), extending another plugin, or including templates from another plugin (WooCommerce, etc.), you can add those text domains as a comma-separated list.
