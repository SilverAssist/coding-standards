# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-08-12

### Added

- Initial `SilverAssist` PHPCS ruleset (PSR-12 + mandatory PHPDoc).
- Initial PHPStan base config (`phpstan/base.neon`, level 8 default).
- CI and release GitHub Actions workflows.
- `AGENTS.md`/`CLAUDE.md`/`copilot-instructions.md` for AI coding agents.

Validated against `senioradvicecom-wp` before release: swapping that
repo's hand-rolled PSR-12 ruleset and PHPStan config for this package
produced byte-for-byte identical `phpcs`/`phpstan` output.
