# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [UNRELEASED](https://github.com/hynek/setup-cached-uv/compare/v1.2.0...main)

### Fixed

- The cache directory couldn't actually be... cached ...due to path restrictions (anymore?) so we moved it into `/tmp/` and made it configurable using the new action input `uv-cache-dir`.
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
