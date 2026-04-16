<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/full-logo/orange/php.png" width="400">
</a></p>

# Valkyrja PHPStan

PHPStan configuration for the Valkyrja project.

## Requirements

- PHP >= 8.4
- [`phpstan/phpstan`](https://github.com/phpstan/phpstan)
  ^2.1.45

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
