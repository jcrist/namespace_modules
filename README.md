# Namespace modules in python

A quick example of how namespace modules work in both python 2 & 3.


## Overview

- ``daskit``

The root namespace module. This could be distributed as part of the main
``dask`` project.

- ``daskit.foo``

A separate package that's installed under the main ``daskit`` namespace.

- ``daskit.bar``

A separate package that's installed under the main ``daskit`` namespace.


## What a package needs to do:

- Have a top-level folder matching the namespace name (in this case ``daskit``)

- Include this line (and only this line) in the top-level ``daskit/__init__.py``

```python
__import__("pkg_resources").declare_namespace(__name__)
```

- Include a subfolder under ``daskit`` with all the module code. This can be a
  regular package.

- A ``setup.py`` looking something like:

```python
from setuptools import setup

setup(name='your_package_name_on_pypi',
      packages=['daskit.your_package_name'],
      namespace_packages=['daskit'],
      zip_safe=False)
```

## Example

After installing the packages above, a user session might look like:

```python

In [1]: import daskit

# Nothing in the top-level namespace
In [2]: [mod for mod in dir(daskit) if not mod.startswith('_')]
Out[2]: []

In [3]: import daskit.foo

# foo is now in the top-level namespace
In [4]: [mod for mod in dir(daskit) if not mod.startswith('_')]
Out[4]: ['foo']

In [5]: daskit.foo.foo(1, 2)
calling from dask.foo
Out[5]: 3

In [6]: from daskit import foo, bar  # Import submodules

In [7]: foo.foo(1, 2)
calling from dask.foo
Out[7]: 3

In [8]: bar.bar(1, 2)
calling from daskit.bar
Out[8]: 3
```
