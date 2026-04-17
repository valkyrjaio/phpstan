<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/long-banner/orange/php.png" width="100%">
</a></p>

# Valkyrja PHPStan

PHPStan configuration for the Valkyrja project.

<p>
    <a href="https://packagist.org/packages/valkyrja/phpstan"><img src="https://poser.pugx.org/valkyrja/phpstan/require/php" alt="PHP Version Require"></a>
    <a href="https://packagist.org/packages/valkyrja/phpstan"><img src="https://poser.pugx.org/valkyrja/phpstan/v" alt="Latest Stable Version"></a>
    <a href="https://packagist.org/packages/valkyrja/phpstan"><img src="https://poser.pugx.org/valkyrja/phpstan/license" alt="License"></a>
    <!-- <a href="https://packagist.org/packages/valkyrja/phpstan"><img src="https://poser.pugx.org/valkyrja/phpstan/downloads" alt="Total Downloads"></a>-->
    <a href="https://scrutinizer-ci.com/g/valkyrjaio/phpstan/?branch=master"><img src="https://scrutinizer-ci.com/g/valkyrjaio/phpstan/badges/quality-score.png?b=master" alt="Scrutinizer"></a>
    <a href="https://coveralls.io/github/valkyrjaio/phpstan?branch=master"><img src="https://coveralls.io/repos/github/valkyrjaio/phpstan/badge.svg?branch=master" alt="Coverage Status" /></a>
    <a href="https://shepherd.dev/github/valkyrjaio/phpstan"><img src="https://shepherd.dev/github/valkyrjaio/phpstan/coverage.svg" alt="Psalm Shepherd" /></a>
    <a href="https://sonarcloud.io/summary/new_code?id=valkyrjaio_phpstan"><img src="https://sonarcloud.io/api/project_badges/measure?project=valkyrjaio_phpstan&metric=sqale_rating" alt="Maintainability Rating" /></a>
</p>

Build Status
------------

<table>
    <tbody>
        <tr>
            <td>Linting</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/phpcodesniffer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/phpcodesniffer.yml/badge.svg?branch=master" alt="PHP Code Sniffer Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/phpcsfixer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/phpcsfixer.yml/badge.svg?branch=master" alt="PHP CS Fixer Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Coding Rules</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/phparkitect.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/phparkitect.yml/badge.svg?branch=master" alt="PHPArkitect Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/rector.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/rector.yml/badge.svg?branch=master" alt="Rector Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Static Analysis</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/phpstan.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/phpstan.yml/badge.svg?branch=master" alt="PHPStan Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/psalm.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/psalm.yml/badge.svg?branch=master" alt="Psalm Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Testing</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpstan/actions/workflows/phpunit.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpstan/actions/workflows/phpunit.yml/badge.svg?branch=master" alt="PHPUnit Build Status"></a>
            </td>
            <td></td>
        </tr>
    </tbody>
</table>

## Usage

Run via the root Composer script:

```bash
composer phpstan
```

This delegates to `vendor/bin/phpstan --memory-limit=-1` inside the CI
directory, using the `phpstan.neon` configuration.

## Configuration

The CI directory ships with a `phpstan.neon` that serves as the reference
configuration. Key settings:

| Setting                     | Value   | Effect                                                         |
|-----------------------------|---------|----------------------------------------------------------------|
| `level`                     | `9`     | Strictest level — all rule categories enabled                  |
| `treatPhpDocTypesAsCertain` | `false` | PHPDoc types are not blindly trusted; prevents false negatives |

### Scanned Paths

| Path   | Included |
|--------|----------|
| `src/` | Yes      |

### Baseline

Known issues are tracked in `phpstan-baseline.neon`. Regenerate it with:

```bash
vendor/bin/phpstan --generate-baseline
```

### Bootstrap

An `autoload.php` is required in the CI directory to bootstrap the project
autoloader before PHPStan analyses the source.

## Workflows

The [`_workflow-call.yml`](.github/workflows/_workflow-call.yml) reusable
workflow runs PHPStan against the calling repository's source. It is designed to
be called from other repositories via `workflow_call`.

### Inputs

| Input                  | Type    | Default                | Description                                                                                                                                           |
|------------------------|---------|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `paths`                | string  | —                      | **Required.** YAML filter spec with two keys: `ci` (CI config files that trigger a base-branch fetch) and `files` (all files that trigger the check). |
| `post-pr-comment`      | boolean | `true`                 | Post a PR comment on failure and remove it on success. Disable when the calling workflow handles its own reporting.                                   |
| `composer-options`     | string  | `''`                   | Extra flags passed to every `composer install` step (e.g. `--ignore-platform-req=ext-openswoole`).                                                    |
| `php-version`          | string  | `'8.4'`                | PHP version to use.                                                                                                                                   |
| `ci-directory`         | string  | `'.github/ci/phpstan'` | Path to the CI directory containing `composer.json` and the tool config.                                                                              |
| `extensions`           | string  | `'mbstring, intl'`     | PHP extensions to install via `shivammathur/setup-php`.                                                                                               |
| `additional-directory` | string  | `''`                   | Path to an additional Composer dependencies directory to install before running PHPStan. Leave empty to skip.                                         |

### Usage

```yaml
jobs:
  phpstan:
    uses: valkyrjaio/phpstan/.github/workflows/_workflow-call.yml@master
    permissions:
      pull-requests: write
      contents: read
    with:
      php-version: '8.4'
      paths: |
        ci:
          - '.github/ci/phpstan/**'
          - '.github/workflows/phpstan.yml'
        files:
          - '.github/ci/phpstan/**'
          - '.github/workflows/phpstan.yml'
          - 'src/**/*.php'
          - 'composer.json'
    secrets: inherit
```

`secrets: inherit` is required to pass the `VALKYRJA_GHA_APP_ID` and
`VALKYRJA_GHA_PRIVATE_KEY` org secrets used for PR comments.
