# Contents

[LuminFire Code Standards](https://git.luminfire.net/ops/tooling/code-standards/phpcs-rulesets)

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

# Initial Setup

Install the code standards below and add them to PHPCS’ `installed_paths`, then clone this repo to get the starter rulesets.

```bash
# Install PHPCS
composer global require "squizlabs/php_codesniffer=*"

# Additional packages required by WP
composer global require phpcsstandards/phpcsutils:@alpha
composer global require phpcsstandards/phpcsextra:@alpha

# create directory to store rulests
mkdir -p ~/codestandards
cd ~/codestandards/

# clone rulesets
git clone git@github.com:PHPCompatibility/PHPCompatibility.git
git clone git@github.com:PHPCompatibility/PHPCompatibilityParagonie.git
git clone git@github.com:PHPCompatibility/PHPCompatibilityWP.git
git clone git@github.com:WordPress-Coding-Standards/WordPress-Coding-Standards.git

# configure PHPCS
phpcs --config-set installed_paths /Users/$(whoami)/codestandards/PHPCompatibility/,/Users/$(whoami)/codestandards/PHPCompatibilityParagonie/PHPCompatibilityParagonieRandomCompat/,/Users/$(whoami)/codestandards/PHPCompatibilityParagonie/PHPCompatibilityParagonieSodiumCompat/,/Users/$(whoami)/codestandards/PHPCompatibilityWP/PHPCompatibilityWP/,/Users/$(whoami)/codestandards/WordPress-Coding-Standards/WordPress/,/Users/$(whoami)/codestandards/WordPress-Coding-Standards/WordPress-Core/,/Users/$(whoami)/codestandards/WordPress-Coding-Standards/WordPress-Docs/,/Users/$(whoami)/codestandards/WordPress-Coding-Standards/WordPress-Extra/

# verify that PHPCS knows about them
phpcs -i

git clone git@git.luminfire.net:ops/tooling/code-standards/phpcs-rulesets.git
```

# Usage

## CLI

Run `phpcs` on the command line to scan all PHP scripts in the current directory. See `phpcs --help` for more information.

Run `phpcbf` on the command line to automatically fix common formatting errors all PHP scripts in the current directory. See `phpcbf --help` for more information.

## IDE

This is the current `phpcs` plugin for Visual Studio Code: [https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs](https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs)

Recommended: in Visual Studio Code, set the default PHPCS standard to the plugin ruleset from this repo so all code is treated as WordPress by default.

- Open VS Code preferences (Code menu > Preferences > Settings), search for `phpcs.showSources`, and enable the checkbox.
- Search for `phpcs.standard` and click the `Edit in settings.json` link and add this line: `"phpcs.standard": "/Users/<your name>/codestandards/phpcs-rulesets/WordPress/plugin/phpcs.xml",`

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
