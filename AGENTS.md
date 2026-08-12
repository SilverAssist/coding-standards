# AGENTS.md — silverassist/coding-standards

Instructions for AI coding agents (Claude Code, GitHub Copilot, Codex, or
any other) picking up work in this repo.

## What this repo is

A Composer package (`type: phpcodesniffer-standard`) shipping:

1. `SilverAssist/ruleset.xml` — a PHPCS ruleset (PSR-12 + mandatory
   PHPDoc via `Squiz.Commenting.ClassComment`/`FunctionComment`).
2. `phpstan/base.neon` — a shareable PHPStan base config (level 8 default).

It has **no WordPress dependencies** and must stay that way — that's the
entire reason it's a separate package from `wp-coding-standards`. If a
task asks you to add a WordPress-specific sniff or dependency here, stop
and flag it: it belongs in `wp-coding-standards` instead, which requires
this package.

Released as v1.0.0, validated against a real consumer before release:
`senioradvicecom-wp` swapped its hand-rolled PSR-12 ruleset and PHPStan
config for this package, and `phpcs`/`phpstan` output was byte-for-byte
identical before and after. `wp-coding-standards` requires this package
(for the shared PHPStan base and to pin tooling versions, not for PHPCS
rule composition — see that package's own docs).

## Ground truth for "is this ruleset correct"

The ruleset was extracted from two real, independently-arrived-at
implementations — not invented from scratch:

- `/Users/miguel/Projects/senioradvicecom-wp/phpcs.xml` and
  `phpstan.neon` (PSR-12, PHPStan level 6 + baseline, strangler-fig
  legacy migration).
- `/Users/miguel/Projects/wp-github-updater/phpcs.xml` and
  `phpstan.neon` (PSR-12 + hand-picked WordPress sniffs, PHPStan level 8).

When in doubt about whether a rule belongs here vs. in `wp-coding-standards`,
check whether both of those reference repos would want it. If only the
WordPress-flavored one would, it doesn't belong here.

## Testing a ruleset change

There's no application code to run — the deliverable *is* the ruleset.
Validate changes by running PHPCS against a fixture with known
violations and asserting the exact violation list, not just "it runs
without crashing." No such fixture suite exists yet under `tests/` —
build one before making a non-trivial ruleset change, don't rely on
downstream consumers to catch a regression.

```bash
composer install
vendor/bin/phpcs -i                          # confirm "SilverAssist" is listed
vendor/bin/phpcs --standard=SilverAssist tests/fixtures/
```

## Commit and PR conventions

Match the conventions already used across Silver Assist repos (see e.g.
`wp-settings-hub`'s history): Conventional Commits style (`feat:`, `fix:`,
`chore:`, `docs:`), semantic versioning tags (`vX.Y.Z`), CHANGELOG.md kept
up to date per [Keep a Changelog](https://keepachangelog.com/).

A ruleset or PHPStan-config change is a **breaking change for every
downstream consumer's CI at once** the moment it's tagged — treat version
bumps accordingly (strict semver, a CHANGELOG entry per rule
addition/removal) and never silently edit an already-tagged version.

## Related repos

- `../wp-coding-standards` — the WordPress-plugin overlay that requires
  this package.
- `../wp-plugin-kernel` — the `LoadableInterface` architecture package
  (unrelated to coding style, but part of the same standardization
  effort).
- `../senioradvicecom-wp` — Phase 2 pilot consumer.
