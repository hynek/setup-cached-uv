# Setup *uv* and Handle Its Cache

> [!CAUTION]
> This action is not ready for production yet.

This action will download and install the latest version of [*uv*](https://github.com/astral-sh/uv) using the official installer and handle its package cache for you.

```yaml
jobs:
  tests:
    runs-on: ubuntu-latest  # or macOS, or Windows
    steps:
      - uses: hynek/setup-cached-uv@v1

      - run: uv ...
```


## Optional Inputs

> [!CAUTION]
> It’s important to understand that once a cache of a certain name has been created, it will be used in all subsequent jobs within the same workflow and will **not** be updated.
>
> This means that if you create the cache in the first job that installs only, say, *build* and *twine*, and later load the cache again and install more packages, those packages will **not** be cached for the next run.

To work around this, *setup-cached-uv* allows you to add two types of suffixes as inputs to the cache name.
This way you can have multiple caches per workflow that contain different sets of packages.

Note that the operating system of the runner is automatically added to the cache name – meaning, without adding suffixes, the names of the caches are `uv-Linux`, `uv-macOS`, and `uv-Windows`.


### `cache-suffix`

A static string suffix to add to the cache name.
This could, for example, be the name of the job.


### `cache-dependency-path`

A path to a file whose contents is hashed and added to the cache name.
May contain glob-style patterns and match more than one file.
Internally, the GitHub Actions function `hashFiles` is used to hash the passed path.


### Examples

Check out [our CI](.github/workflows/ci.yml) to see both inputs in action.


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
