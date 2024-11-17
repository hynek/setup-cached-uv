#  Setup *uv* and Handle Its Cache

> [!NOTE]
> Note that there's now an official [*astral-sh/setup-uv*](https://github.com/astral-sh/setup-uv) action.
>
> As far as we can tell, the cache-handling of this action is currently more straight-forward, though.

This action will download and install the latest version of [*uv*](https://github.com/astral-sh/uv) using the official installer and handle its package cache for you.

```yaml
jobs:
  tests:
    runs-on: ubuntu-latest  # or macOS, or Windows
    steps:
      - uses: hynek/setup-cached-uv@v2

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

To work around this, *setup-cached-uv* gives you several ways to invalidate the cache by adding suffixes to the cache key.
This way you can have multiple caches per workflow that contain different sets of packages and have them expire periodically, even if in constant use.

By default, the operating system of the runner, the name of the workflow, the job name, and the current calendar week are automatically added to the cache key.
Meaning, without adding suffixes yourself, the cache keys look something like `uv-Linux-CI-tests-30`, `uv-macOS-CI-tests-30`, and `uv-Windows-CI-tests-30`.

To keep the caches small, this action automatically runs `uv cache prune --ci` before saving the cache.
This keeps the files that have been downloaded, but removes any temporary files that are not needed for the cache to work, and that will only slow cache operations down.


### Optional Inputs

#### `cache-dependency-path`

A path to a file whose contents is hashed and appended to the cache name.
May contain glob-style patterns and match more than one file.
Internally, the GitHub Actions function [`hashFiles`](https://docs.github.com/en/actions/learn-github-actions/expressions#hashfiles) is used to hash the passed path.

Using this with a fully pinned [`uv.lock`](https://docs.astral.sh/uv/concepts/projects/) or `requirements.txt` file is the most efficient use of this action because it automatically invalidates the cache when your dependencies or their versions change.

You may want to set `cache-date-suffix` to `""` if you use this input with an all-encompassing lockfile.

The default is `pyproject.toml` which invalidates the cache whenever a modern Python package changes its packaging metadata, but is not enough to be the only driver for cache invalidation.


#### `cache-suffix`

A static string to append to the cache key.
This could, for example, be the Python version if the dependencies differ significantly between versions.


#### `cache-date-suffix`

If not empty, it’s interpreted as an argument to [`date`](https://man7.org/linux/man-pages/man1/date.1.html) and its output is appended to the cache key.

The default is `+%V` which is the calendar week number.
This means that the cache is refreshed weekly.

You may want to set this to `""` if `cache-dependency-path` is enough to invalidate the cache (in other words: it's pointing to a complete lockfile).


#### `uv-cache-path`

The path to *uv*’s cache.
Due to path restrictions, it’s impossible to cache the default path, so we moved it to a path beneath `/tmp` for Linux/macOS and `D:\a\_temp` on Windows.
You can change it to elsewhere using this input, but make sure that [*actions/cache*](https://github.com/actions/cache) can find it.


#### `if-use-cache`

This defaults to `true`, but can be used to disable the cache, since GitHub’s default caching speed can be slower than an uncached *uv*.
For example, if you have dependencies that don’t provide prebuilt PyPy wheels, you can only cache that run like this:

```yaml
      # ...
      - uses: hynek/setup-cached-uv@v2
        with:
          if-use-cache: ${{ startsWith(matrix.python-version, 'pypy') }}
      # ...
```


### Examples

Check out [our CI](.github/workflows/ci.yml) to see some inputs in action.


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
