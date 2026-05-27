# Laravel Nodeless

Laravel Nodeless is a Laravel starter kit for building Livewire applications without a frontend build system.

It starts from Laravel's official Livewire starter kit, then removes Node, npm, Vite, Tailwind compilation, and all related CI steps. The result is a PHP-first application scaffold that can be installed, developed, tested, and deployed with Composer and Laravel tooling only.

## Philosophy

Laravel is productive because the framework gives you a cohesive, batteries-included path for building server-rendered applications. This starter kit keeps that path intentionally small:

- No Node runtime requirement.
- No npm install step.
- No Vite dev server.
- No frontend build step in local development, CI, or deployment.
- Livewire and Flux remain the primary UI layer.
- Static CSS, JavaScript, and fonts are checked in under `public/build` and served directly by Laravel.

This tradeoff is deliberate. You give up an editable Tailwind/Vite pipeline in exchange for a starter kit that works in PHP-only environments and has fewer moving parts.

## What's Included

- Laravel 13
- Livewire 4
- Flux 2
- Fortify authentication
- Two-factor authentication
- Passkey support
- Prebuilt Tailwind/Flux assets served from `public/build`

## What's Removed

- `package.json`
- `package-lock.json`
- `node_modules`
- `vite.config.js`
- `resources/css/app.css`
- `resources/js/*`
- npm and Vite steps from Composer scripts and GitHub Actions

## Getting Started

Install dependencies and prepare the application:

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
```

Run the development server:

```bash
php artisan serve
```

Run the test suite:

```bash
php artisan test
```

## Working With Assets

The starter kit intentionally does not include a source asset pipeline. Application CSS, font CSS, fonts, and the passkey browser helper are committed as static files in `public/build/assets`.

If you need a heavily customized visual system, this may not be the right starter kit yet. The goal is to support Laravel apps that are happy with the included Livewire/Flux baseline and prefer zero frontend build tooling.

## Repository

This project lives at `artisan-build/laravel-nodeless`.
