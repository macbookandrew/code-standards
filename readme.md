# Contents

[LuminFire Code Standards](https://git.luminfire.net/ops/tooling/code-standards/phpcs-rulesets)

- [Contents](#contents)
- [About](#about)
- [Initial Setup](#initial-setup)
- [Usage](#usage)
  - [CLI](#cli)
  - [IDE](#ide)
- [Whitelisting](#whitelisting)
- [Frameworks](#frameworks)
  - [Laravel](#laravel)
  - [WordPress](#wordpress)
    - [Project Setup](#project-setup)
      - [Minimum Versions](#minimum-versions)
      - [Translations](#translations)

# About

[PHP Code Sniffer](https://github.com/squizlabs/PHP_CodeSniffer) is a set of two PHP scripts; the main `phpcs` script that tokenizes PHP, JavaScript and CSS files to detect violations of a defined coding standard, and a second `phpcbf` script to automatically correct coding standard violations. PHP_CodeSniffer is an essential development tool that ensures your code remains clean and consistent.

[Larastan](https://github.com/nunomaduro/larastan) is a static analyzer for Laravel apps. See that project for more documentation.

# Initial Setup

Install the code standards below and add them to PHPCS’ `installed_paths`, then clone this repo to get the starter rulesets.

```bash
# Install PHPCS
composer global require "squizlabs/php_codesniffer=*"

# Install plugin manager (eliminates need to manually set installed_paths)
composer global require dealerdirect/phpcodesniffer-composer-installer

# Install sniffs
composer global require wp-coding-standards/wpcs \
    phpcompatibility/php-compatibility \
    phpcompatibility/phpcompatibility-paragonie \
    phpcompatibility/phpcompatibility-wp \
    pheromone/phpcs-security-audit


# Verify that PHPCS knows about them
phpcs -i

# Expect to a list like this including several PHPCompatibility and WordPress entries:
# The installed coding standards are PEAR, Zend, PSR2, MySource, Squiz, PSR1, PSR12, PHPCompatibility, PHPCompatibilityParagonieRandomCompat, PHPCompatibilityParagonieSodiumCompat, PHPCompatibilityWP, WordPress, WordPress-Extra, WordPress-Docs and WordPress-Core

# Clone this repo with starter project rulesets.
```

# Usage

## CLI

Run `phpcs` on the command line to scan all PHP scripts in the current directory. See `phpcs --help` for more information.

Run `phpcbf` on the command line to automatically fix common formatting errors all PHP scripts in the current directory. See `phpcbf --help` for more information.

## IDE

This is the current `phpcs` plugin for Visual Studio Code: [https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs](https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs)

Recommended global setting for WordPress developers: in Visual Studio Code, set the default PHPCS standard to the plugin ruleset from this repo so all code is treated as WordPress by default.

- Open VS Code preferences (Code menu > Preferences > Settings), search for `phpcs.showSources`, and enable the checkbox.
- Search for `phpcs.standard` and click the `Edit in settings.json` link and add this line: `"phpcs.standard": "{path to this repo}/WordPress/plugin/phpcs.xml",`

# Whitelisting

Don’t whitelist errors or warnings just to get rid of the problem! Fix it instead. 🙂

If there is a good reason to whitelist a specific line, add this to the end of the line: `// phpcs:ignore <offending rule code, shown by IDE> -- <add comment here explaining why it’s whitelisted>`

On occasion, you may need to disable an error or class of errors for multiple lines; in that case, add these lines:

```php
// phpcs:disable <offending rule code>
// Code here
// phpcs:enable <offending rule code>
```

# Frameworks

## Laravel

See [usage](#usage) above; there is no special setup required.

For static analysis, copy `Laravel/phpstan.neon.dist` to your project and run `composer require --dev nunomaduro/larastan`. Suggested VS Code extension: [PHPStan by swordev](https://marketplace.visualstudio.com/items?itemName=swordev.phpstan)

For Tighten Lint, copy `Laravel/tlint.json` to your project. Suggested VS Code extension: [tighten-lint by David Walker](https://marketplace.visualstudio.com/items?itemName=d9705996.tighten-lint)

## WordPress

Read about the [WordPress Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/).

### Project Setup

Generally, ignore errors and warnings in third-party code.

To use the custom WordPress rulesets in first-party plugins or themes, copy the appropriate `phpcs.xml` file from this repo into the custom plugin or theme directory. Edit the file as described below under [Translations](#translations).

VS Code will use the ruleset to scan files as you work on them. To scan the whole directory at once, see [CLI](#cli) above.

#### Minimum Versions

By default, PHP 7.0 is set as the minimum supported version, and deprecated functions will be flagged.

By default, WordPress 4.8 is set as the minimum supported version, and deprecated functions will be flagged.

#### Translations

Set the `WordPress.WP.I18n` rule’s `text_domain` property to your plugin/theme’s text domain. If you’re creating a child theme (Genesis), extending another plugin, or including templates from another plugin (WooCommerce, etc.), you can add those text domains as a comma-separated list.
