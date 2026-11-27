# wp-utilities-dev

Shared development scripts for [thefrosty/wp-utilities](https://github.com/thefrosty/wp-utilities) and the plugins
and packages built on it. They used to ship inside wp-utilities' `bin/` directory; they live here so they can be a
`require-dev` dependency and stay out of production installs.

## Installation

```bash
composer require --dev thefrosty/wp-utilities-dev
```

Composer links each script into your `vendor/bin` directory.

## Scripts

| Script | What it does |
|---|---|
| `phpcs.sh` | Runs `phpcs` on the PHP files changed against the default branch. |
| `phpcbf.sh` | Runs `phpcbf` on the PHP files changed against the default branch. |
| `phpstan.sh` | Runs `phpstan analyze` on changed PHP files (excluding `tests/`). |
| `psalm.sh` | Runs `psalm` on changed PHP files (excluding `tests/`). |
| `eslint.sh` | Runs `npx standard` on changed JS/JSX files. |
| `install-wp-tests.sh` | Installs WordPress and the WordPress PHPUnit test suite (from wp-cli's scaffold). |
| `create-all-branches.sh` | In CI, fetches and creates local tracking branches for every remote branch. |

The PHP and JS scripts only lint files changed against the default branch (`develop` unless `--default-branch=…`
is passed). Set `CHANGED_FILES` to a space-separated list to lint exactly those files instead, and `PHP_VERSION`
(default `8.4`) for the PHPCompatibility `testVersion`. Each one calls the tool from `./vendor/bin` first.

## Usage

`composer.json`:

```json
"scripts": {
  "phpcs": "bash ./vendor/bin/phpcs.sh --standard=phpcs-ruleset.xml",
  "phpcs:fix": "bash ./vendor/bin/phpcbf.sh --standard=phpcs-ruleset.xml",
  "phpstan": "bash ./vendor/bin/phpstan.sh",
  "psalm": "bash ./vendor/bin/psalm.sh"
}
```

GitHub Actions (after `composer install`):

```yaml
- name: Setup WordPress
  run: bash ./vendor/bin/install-wp-tests.sh wordpress_test root root 127.0.0.1:3306 latest true
```

## Migrating from `vendor/thefrosty/wp-utilities/bin`

wp-utilities v4 no longer ships `bin/`. Add this package with `composer require --dev thefrosty/wp-utilities-dev`
and replace `./vendor/thefrosty/wp-utilities/bin/<script>.sh` with `./vendor/bin/<script>.sh` in your
`composer.json`, `package.json` and CI workflows.
