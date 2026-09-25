# Silver Assist Coding Standards

Framework-agnostic PHPCS ruleset (PSR-12 + mandatory PHPDoc) and a PHPStan
base config, shared by every Silver Assist PHP project — WordPress plugin
or not.

This package intentionally has **zero WordPress dependencies**. WordPress
plugins should require [`silverassist/wp-coding-standards`](https://github.com/SilverAssist/wp-coding-standards)
instead, which requires this package and layers WordPress-Extra, i18n, and
naming-convention sniffs on top.

## Install

```bash
composer require --dev silverassist/coding-standards
```

## Installing via Composer (private repository)

SilverAssist packages are installed from their GitHub repositories with a Composer
`vcs` repository, not from Packagist.org. Those repositories can require
authentication, so always configure a token.

1. **Declare the repository** in your project's root `composer.json`. Composer only
   reads `repositories` from the root package, so every SilverAssist package your
   project needs must be listed there, including transitive ones:

   ```json
   {
     "repositories": [
       { "type": "vcs", "url": "https://github.com/SilverAssist/coding-standards" }
     ],
     "require-dev": {
       "silverassist/coding-standards": "^1.0"
     }
   }
   ```

2. **Authenticate** with a GitHub token that can read the repository:
   - Locally: `composer config --global github-oauth.github.com <token>`
   - CI: set `COMPOSER_AUTH='{"github-oauth":{"github.com":"<token>"}}'` from a secret
     (a GitHub Actions secret or a Bitbucket variable).

   Never commit a token or an `auth.json`. Without a token, Composer hits GitHub's
   anonymous API limit (60 requests per hour per IP) and falls back to an SSH clone.

3. **Refresh the lock file** if your project commits `composer.lock`: run
   `composer update --lock` after adding `repositories`, so the lock file's
   `content-hash` matches `composer.json`.

### Troubleshooting

| Symptom | Cause |
|---------|-------|
| `Failed to clone the git@github.com:SilverAssist/coding-standards.git repository, try running in interactive mode...` followed by `Permission denied (publickey)` | The token is missing or has no access to the repository. |
| `remote: Invalid username or token` | The token is invalid or expired. |
| `it could not be found in any version` | The `vcs` entry is missing from the root `composer.json`. |

## Usage — PHPCS

Create a `phpcs.xml` in your project (this package never scans files
itself — it only ships the rule definitions):

```xml
<?xml version="1.0"?>
<ruleset name="YourProject">
    <arg name="colors"/>
    <arg value="p"/>
    <config name="php_version" value="80200"/>

    <file>src</file>

    <rule ref="SilverAssist"/>
</ruleset>
```

Run it:

```bash
vendor/bin/phpcs
```

### Migrating a legacy codebase

If PHPDoc type-hint enforcement is too strict for code that hasn't been
typed yet (see `senioradvicecom-wp`'s strangler-fig migration), exclude
just those two sub-sniffs locally:

```xml
<rule ref="SilverAssist">
    <exclude name="Squiz.Commenting.FunctionComment.TypeHintMissing"/>
    <exclude name="Squiz.Commenting.FunctionComment.ScalarTypeHintMissing"/>
</rule>
```

## Usage — PHPStan

```yaml
# phpstan.neon
includes:
    - vendor/silverassist/coding-standards/phpstan/base.neon

parameters:
    paths:
        - src
    bootstrapFiles:
        - bootstrap.php
```

Legacy projects that can't yet run at level 8 can override it after the
include:

```yaml
includes:
    - vendor/silverassist/coding-standards/phpstan/base.neon

parameters:
    level: 6
    paths:
        - src
    includes:
        - phpstan-baseline.neon
```

## What's deliberately NOT here

- WordPress sniffs (`WordPress-Extra`, i18n, escaping/sanitization,
  `PrefixAllGlobals`) — see `silverassist/wp-coding-standards`.
- The `LoadableInterface` / `Plugin::instance()` bootstrap pattern — see
  `silverassist/wp-plugin-kernel`.
- CI/CD workflow templates — those live in `silverassist/wp-coding-standards`
  too, since the reference implementations (WordPress Test Suite matrix,
  release ZIP building) are WordPress-plugin specific. A non-WP project like
  `senioradvicecom-wp` runs PHPCS/PHPStan/PHPUnit directly in its own
  pipeline instead of via a reusable workflow.

## See also

- [`AGENTS.md`](AGENTS.md) — instructions for AI coding agents working in
  this repo.
