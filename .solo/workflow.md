# Workflow — laravel-nodeless

Project profile for the `multi-agent-build` skill. The coordinator agent reads this FIRST.
Keep it truthful as the project evolves.

Note: this repo IS the `artisan-build/laravel-nodeless` starter kit (local folder `nodeps` is a
legacy name). Changes here propagate to every project spawned from it — hold the template to a
higher hygiene bar than a normal app (no machine-specific paths, no personal config in git).

## Phase & mode
- phase: launched (published starter kit, maintained incrementally)
- default mode: A-autonomous
- merge_policy: merge when CI green; no PR code review
- merge method: `gh pr merge --squash --auto` (auto-delete branch is OFF on the repo; delete manually)

## Hard gate (must be green before review; coordinator verifies on the committed SHA, clean tree)
- command: `composer ready` (ide-helper regen + rector + pint + phpstan + pest + composer audit)
- extra suites: none
- monorepo: no

## CI (the merge gate for Mode A)
- status: verified
- minimum bar: CI MUST include (1) testing and (2) static analysis. If it doesn't, don't gate merges
  on CI (Mode A) until fixed.
- workflows/jobs: `.github/workflows/tests.yml` — `composer stan` (PHPStan/larastan level 6) then
  `./vendor/bin/pest`, PHP 8.5 matrix; `.github/workflows/lint.yml` — `composer lint` (Pint).
  Both configure the Flux Pro composer credential (`http-basic.composer.fluxui.dev`) from repo
  secrets `FLUX_USERNAME` / `FLUX_LICENSE_KEY` — known and accepted, not a problem.

## Dependency install (fresh worktree)
- command: `composer install --no-interaction --prefer-dist`
  (requires Flux Pro credential for `composer.fluxui.dev`; local machines have it in global
  composer auth — never commit an `auth.json`)
- post-install: `cp .env.example .env && php artisan key:generate` (tests use in-memory sqlite via
  phpunit.xml; `touch database/database.sqlite` only if running artisan commands that hit the DB)

## Harness map (role -> runtime; decorrelate model lineages)
- implementer: OpenCode (Solo agent_tool_id 2)
- quality reviewer: Fable (Solo agent_tool_id 10)
- acceptance judge: Claude (Solo agent_tool_id 3)

Codex is deliberately NOT in this map any more. Codex 0.147.0 self-updates on spawn, and the updated
TUI then dies under Solo's PTY (`No PTY available`) — a Codex-backed role hangs instead of reviewing.
This map is the standing fleet-wide assignment; do not revert the reviewer to Codex without first
confirming that bug is gone.

## Toolchain conformance — the ride-along rule (STANDING, all projects)

Run the project's full conformance command (`composer ready`, or the stack equivalent) as part of
FINALIZING every PR, and **let whatever it changes ride along in that PR** as a single isolated commit
titled `composer ready`.

The point is that conforming to the current standard is **passive** rather than something anyone has to
remember. Tools like Rector exist to keep the codebase at the current standard continuously; if their
output only lands when someone thinks to run them, the codebase drifts and the eventual catch-up is a huge
unreviewable diff.

- **Do NOT open a separate branch for these changes.** As long as the tool CONFIGURATION is unchanged, the
  unrelated changes riding along on any given PR are small.
- **The one exception:** introducing a new Rector rule, or changing `pint.json` / equivalent tool config.
  That sweep is large and deliberate, so it gets its own dedicated branch and PR.
- Keep it in its OWN commit so a reviewer can separate "the feature" from "the sweep" at a glance.

## Ship details
- branch naming: feat/<slug> (fix/<slug> for fixes, chore/<slug> for maintenance)
- PR target repo: artisan-build/laravel-nodeless
- release / split steps: none (consumed via `composer create-project` / starter-kit installer)

## Plan & coordination
- plan location: per-build Solo scratchpad (no standing PRD)
- Solo project: nodeps (7)
- run-log: per-build scratchpad named `<branch>-run-log`; coordinator appends at every transition

## Stack notes / quirks
- Nodeless by design: no Node, npm, Vite, or frontend build step. Do not introduce any.
- Livewire 4 + Flux Pro 2 on Laravel 13; PHP ^8.3 required, CI exercises 8.5.
- `composer ready` regenerates IDE helper files. `_ide_helper.php` / `_ide_helper_models.php` stay
  COMMITTED on purpose — PHPStan `scanFiles` needs `_ide_helper_models.php` to resolve model types.
  `.phpstorm.meta.php` is gitignored (it embeds machine-specific absolute paths); the asymmetry is
  deliberate — do not "fix" it by ignoring all three or committing all three.
- PHPStan baseline lives in `phpstan-baseline.neon`; keep it shrinking, never grow it silently.
