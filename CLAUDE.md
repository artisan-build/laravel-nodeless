# laravel-nodeless

This repo is the `artisan-build/laravel-nodeless` starter kit: a Laravel Livewire starter kit with
no Node, npm, Vite, or frontend build step. Do not introduce a frontend build step or Node tooling.

Changes here are inherited by every project spawned from this kit — keep the template clean of
machine-specific paths and personal config.

## Workflow

Feature builds: see `.solo/workflow.md` and the `multi-agent-build` skill.

Hard gate before any PR: `composer ready` (ide-helper + rector + pint + phpstan + pest + audit).

## IDE helper files

`_ide_helper.php` and `_ide_helper_models.php` are committed on purpose (PHPStan scans
`_ide_helper_models.php`). `.phpstorm.meta.php` is gitignored on purpose (it embeds absolute local
paths). This asymmetry is deliberate — do not make them consistent in either direction.
