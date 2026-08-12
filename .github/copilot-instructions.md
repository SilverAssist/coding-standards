# GitHub Copilot Instructions for Silver Assist Coding Standards

## Project Context

This is **silverassist/coding-standards**, a Composer package providing a
framework-agnostic PHPCS ruleset and PHPStan base config for all Silver
Assist PHP projects — WordPress plugin or not.

### Purpose

- Give every Silver Assist PHP repo (WordPress plugin or plain PHP app) a
  single, versioned PSR-12 + PHPDoc baseline instead of hand-copied,
  slowly-drifting `phpcs.xml`/`phpstan.neon` files.
- Stay WordPress-free so non-WP projects (`senioradvicecom-wp`) don't have
  to pull in WordPress stubs/sniffs just for basic PHP style checking.

### Architecture

- **Deliverable**: no application code — the package *is* configuration
  (`SilverAssist/ruleset.xml`, `phpstan/base.neon`).
- **Type**: `phpcodesniffer-standard` (installed via
  `dealerdirect/phpcodesniffer-composer-installer`).
- **License**: PolyForm Noncommercial 1.0.0
- **Namespace**: none — this package has no PHP classes.

## Code Standards

- Any PHP fixture files added under `tests/` for ruleset validation must
  themselves be intentionally minimal — they exist to exercise specific
  sniffs, not to demonstrate real application code.
- XML in `SilverAssist/ruleset.xml` must stay valid PHPCS ruleset XML;
  validate with `vendor/bin/phpcs -i` after any edit.

## File Structure

```
coding-standards/
├── SilverAssist/
│   └── ruleset.xml         # PSR-12 + PHPDoc enforcement
├── phpstan/
│   └── base.neon           # Level 8 default, override-friendly
├── tests/                  # Fixture files for ruleset validation (none yet)
├── composer.json
├── AGENTS.md                # Full instructions for AI coding agents
└── README.md                # Consumer-facing usage docs
```

## Development Workflow

1. Any change to `SilverAssist/ruleset.xml` or `phpstan/base.neon` is a
   breaking change for every downstream consumer the moment it's tagged.
   Validate against a real consumer (e.g. `senioradvicecom-wp`) before
   tagging a new version.
2. Update `CHANGELOG.md` per [Keep a Changelog](https://keepachangelog.com/)
   for every rule addition, removal, or behavior change.

## Related Projects

- **wp-coding-standards**: requires this package, adds the WordPress-Extra
  overlay for the 7 WordPress plugin repos.
- **wp-plugin-kernel**: the `LoadableInterface` architecture package —
  unrelated to coding style, same standardization initiative.
- **senioradvicecom-wp**: the non-WordPress pilot consumer.
