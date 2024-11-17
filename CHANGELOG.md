# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased](https://github.com/hynek/setup-cached-uv/compare/v2.3.0...main)


## [2.3.0](https://github.com/hynek/setup-cached-uv/compare/v2.2.1...v2.3.0)

### Changed

- The default value for `cache-dependency-path` is now `pyproject.toml`.
  This means that changes to packaging metadata of modern Python packages will invalidate the cache.
  [#19](https://github.com/hynek/setup-cached-uv/pull/19)


## [2.2.1](https://github.com/hynek/setup-cached-uv/compare/v2.2.0...v2.2.1)

### Fixed

- Quoting around cache dir handling.
  [#18](https://github.com/hynek/setup-cached-uv/pull/18)


## [2.2.0](https://github.com/hynek/setup-cached-uv/compare/v2.1.0...v2.2.0)

### Changed

- The default cache path for Windows is now `D:\a\_temp\scu-uv-cache`.
  [#16](https://github.com/hynek/setup-cached-uv/pull/16)


## [2.1.0](https://github.com/hynek/setup-cached-uv/compare/v2.0.0...v2.1.0) - 2024-08-15

### Changed

- When installing on Linux and macOS, it is now enforced to use TLS 1.2 to protect against downgrade attacks.
  [#14](https://github.com/hynek/setup-cached-uv/pull/14)


## [2.0.0](https://github.com/hynek/setup-cached-uv/compare/v1.3.0...v2.0.0) - 2024-07-26

### Added

- Automatic cache pruning.
  If caching is active, we now automatically run `uv cache prune --ci` that shrinks the cache directory to only downloaded files.
  This make the cache smaller and therefore faster to save and restore.
  [#9](https://github.com/hynek/setup-cached-uv/pull/9)

- The current calendar week is now added to the cache key by default.
  This means that the cache is refreshed weekly.

  You can tweak the behavior using the `cache-date-suffix` input.
  Setting it to `""` disables this feature, any other value is interpreted as an argument to the `date` CLI command.
  [#11](https://github.com/hynek/setup-cached-uv/pull/11)


### Changed

- The name of the workflow and of the current job are now part of the cache key.
  While this means that you can't share a cache between jobs, this should be only a minor inconvenience in practice and make it do the right thing in the vast majority of cases.

  If this is a problem for you, please open an issue and tell us about your use-case.
  We can always add an option to set the whole key explicitly.
  [#10](https://github.com/hynek/setup-cached-uv/pull/10)


## [1.3.0](https://github.com/hynek/setup-cached-uv/compare/v1.2.0...v1.3.0) - 2024-07-26

### Fixed

- The cache directory couldn't actually be... cached ...due to path restrictions (anymore?) so we moved it into `/tmp/` and made it configurable using the new action input `uv-cache-path`.
  [#8](https://github.com/hynek/setup-cached-uv/pull/8)


## [1.2.0](https://github.com/hynek/setup-cached-uv/compare/v1.1.0...v1.2.0) - 2024-07-22

### Added

- Option to conditionally disable caching by setting `if-use-cache` to anything else than `'true'`.

  *uv* is increadibly fast at installing wheels, while GitHub Actions's caching is rather slow.
  But it's still faster than building missing wheels for exotic platforms (say, PyPy).

  This option allows you activate caching only when it's helpful.
  [#6](https://github.com/hynek/setup-cached-uv/pull/6)


## [1.1.0](https://github.com/hynek/setup-cached-uv/compare/v1.0.0...v1.1.0) - 2024-04-08

### Added

- Support for multiple caches per workflow by adding two types of suffixes.
  [#2](https://github.com/hynek/setup-cached-uv/pull/2)


## [1.0.0](https://github.com/hynek/setup-cached-uv/tree/v1.0.0) - 2024-03-31

### Added

- Everything.
