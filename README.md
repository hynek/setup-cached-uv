# Setup *uv* and Handle Its Cache

> [!WARNING]
> This action is still semi-experimental.
> I’m happy to [hear feedback](https://github.com/hynek/setup-cached-uv/issues), though!

This action will download and install the latest version of [*uv*](https://github.com/astral-sh/uv) using the official installer and handle its package cache for you.

```yaml
jobs:
  tests:
    runs-on: ubuntu-latest  # or macOS, or Windows
    steps:
      - uses: hynek/setup-cached-uv@v1

      - run: uv ...
```


## Cache Management

> [!CAUTION]
> It’s important to understand that once a cache of a certain key has been created, it will be used in all subsequent jobs within the same workflow and will **not** be updated.
>
> This means that if you create the cache in the first job that installs only, say, *build* and *twine*, and later load the cache again and install more packages, those packages will **not** be cached for the next run.
>
> A cache also [doesn’t expire as long as it has been used in the last 7 days](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy).
> That does **not** mean that you’re getting obsolete package versions – just that they’re not cached.

To work around this, *setup-cached-uv* allows you to add two types of suffixes as inputs to the cache key.
This way you can have multiple caches per workflow that contain different sets of packages.

Note that the operating system of the runner, the name of the workflow, and the job name are automatically added to the cache key – meaning, without adding suffixes, the names of the caches are something like `uv-Linux-CI-tests`, `uv-macOS-CI-tests`, and `uv-Windows-CI-tests`.


### Optional Inputs

#### `cache-suffix`

A static string to append to the cache key.
This could, for example, be the current week number to refresh the cache weekly:

```yaml
      # ...
      - run: echo WEEK=$(date +%V) >>$GITHUB_ENV
        shell: bash

      - uses: hynek/setup-cached-uv@v1
        with:
          cache-suffix: ${{ env.WEEK }}
      # ...
```


#### `cache-dependency-path`

A path to a file whose contents is hashed and appended to the cache name.
May contain glob-style patterns and match more than one file.
Internally, the GitHub Actions function `hashFiles` is used to hash the passed path.

Using this with a fully pinned `requirements.txt` file is the most efficient use of this action because it automatically invalidates the cache.


#### `uv-cache-path`

The path to *uv*'s cache.
Due to path restrictions, it's impossible to cache the default path, so we moved it to a path beneath `/tmp`.
You can change it to elsewhere using this input, but make sure that [*actions/cache*](https://github.com/actions/cache) can find it.


#### `if-use-cache`

This defaults to `true`, but can be used to disable the cache, since GitHub's default caching speed can be slower than an uncached *uv*.
For example, if you have dependencies that don't provide prebuilt PyPy wheels, you can only cache that run like this:

```yaml
      # ...
      - uses: hynek/setup-cached-uv@v1
        with:
          if-use-cache: ${{ startsWith(matrix.python-version, 'pypy') }}
      # ...
```


### Examples

Check out [our CI](.github/workflows/ci.yml) to see some inputs in action.


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
