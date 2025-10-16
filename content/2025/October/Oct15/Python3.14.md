We know that python has released the latest version [3.14](https://x.com/charliermarsh/status/1975913762344608129?s=46) recently, a free-threaded (or no-GIL) version of the Python interpreter.

When I ran some really simple pytorch examples with uv, and set the required python version to >=3.12 in pyproject.toml, I got the following error:

```bash
!!
    check.warn(importable)
  /root/.cache/uv/builds-v0/.tmpWjh1Dv/lib/python3.14/site-packages/setuptools/command/build_py.py:212: _Warning: Package 'pyarrow.vendored' is absent from the `packages` configuration.
  !!
...
```
and even more errors, which probably means that python3.14 is not fully supported by many packages yet.

After switching back to python3.13, everything works fine again as 3.12 is way too old.
