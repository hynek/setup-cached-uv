# Setup uv and Handle Its Cache

This action will download and install the latest version of [*uv*](https://github.com/astral-sh/uv) using the official installer and handle its package cache for you.

```yaml
jobs:
  tests:
    runs-on: ubuntu-latest  # or macOS, or Windows
    steps:
      - uses: hynek/setup-cached-uv@v1

      - run: uv ...
```


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
