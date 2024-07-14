### pyproject.toml vs setup.py

Consider the case where build backend is set to `setuptools.build_meta` in `pyproject.toml`.
When `[project]` table in `pyproject.toml` and `setup.py` is prepared, what happens?

### Anser: `pyproject.toml` won!

> pyproject.toml

```toml
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project]
name = "test-prj"
dynamic = ["version"]
```

> setup.py

```python
from setuptools import setup

setup(name="test_prj_setup_py")
```

### Result

```
> pip install .
Processing path/to/test-prj
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: test-prj
  Building wheel for test-prj (pyproject.toml) ... done
  Created wheel for test-prj: filename=test_prj-0.0.0-py3-none-any.whl size=1126 sha256=14a6c639c9f9b75cae7ede2ba4f17278f9870e276b16468ffb3607f3d5572338
  Stored in directory: /path/to/cache-dir/pip/wheels/path/to/cache
Successfully built test-prj
Installing collected packages: test-prj
  Attempting uninstall: test-prj
    Found existing installation: test-prj 0.0.0
    Uninstalling test-prj-0.0.0:
      Successfully uninstalled test-prj-0.0.0
Successfully installed test-prj-0.0.0
```

> [!NOTE]
> If `[project]` section is removed, pip installed the package as `test_prj_setup_py` (i.e. `setup.py` is picked up).

> [!NOTE]
> If `[build-system]` is not defined in `pyproject.toml` and `setup.py` does not exist, `setuptools` is automatically used and read metadata in `[project]`.
