# Python Packaging and Distribution

Packaging and distributing your Python code allows others to easily install and use your projects. This involves creating a distributable archive (like a wheel or sdist) and often publishing it to a package index like PyPI (Python Package Index).

## Key Concepts

-   **Package:** A directory containing Python modules and an `__init__.py` file, making it importable.
-   **Module:** A single Python file (`.py`).
-   **Distribution Package:** The archive file (`.whl`, `.tar.gz`) that gets uploaded to PyPI and installed by `pip`.
-   **PyPI (Python Package Index):** The official third-party software repository for Python.
-   **`setuptools`:** A library that makes it easier to build and distribute Python packages.
-   **`wheel`:** A built-package format that is faster to install than source distributions (`sdist`).
-   **`twine`:** A utility for securely uploading Python packages to PyPI.

## Project Structure

A typical project structure for a distributable package looks like this:

```
my_package_name/
├── src/
│   └── my_package_name/
│       ├── __init__.py
│       ├── module1.py
│       └── module2.py
├── tests/
│   ├── test_module1.py
│   └── test_module2.py
├── pyproject.toml
├── README.md
├── LICENSE
└── .gitignore
```

-   `src/my_package_name/`: This is the actual Python package code. Using a `src/` directory is a modern best practice.
-   `pyproject.toml`: The modern, standardized way to configure Python projects, replacing `setup.py` for many tasks.
-   `README.md`: Provides information about your project.
-   `LICENSE`: Specifies the licensing terms for your project.

## 1. `pyproject.toml` (Modern Configuration)

`pyproject.toml` is a configuration file introduced by PEP 518 and PEP 621. It's becoming the standard for defining build system requirements and project metadata.

Here's a basic example:

```toml
# pyproject.toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "my-awesome-package"
version = "0.1.0"
authors = [
  { name="Your Name", email="your.email@example.com" },
]
description = "A short description of my awesome package."
readme = "README.md"
requires-python = ">=3.8"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
keywords = ["utility", "example"]

[project.urls]
"Homepage" = "https://github.com/yourusername/my-awesome-package"
"Bug Tracker" = "https://github.com/yourusername/my-awesome-package/issues"

[project.optional-dependencies]
dev = ["pytest", "twine", "build"]

[project.scripts]
my-cli-tool = "my_package_name.module1:main_function"

[tool.setuptools.packages]
find = {"where": ["src"]}
```

**Explanation of sections:**
-   `[build-system]`: Specifies the build backend (e.g., `setuptools`).
-   `[project]`: Contains core metadata about your project (name, version, authors, description, etc.).
-   `[project.urls]`: Links to your project's homepage, bug tracker, etc.
-   `[project.optional-dependencies]`: Defines extra sets of dependencies (e.g., `dev` for development tools).
-   `[project.scripts]`: Defines command-line entry points for your package.
-   `[tool.setuptools.packages]`: Specific to `setuptools`, tells it where to find your package code (e.g., in the `src` directory).

## 2. Building the Distribution Package

First, ensure you have the `build` package installed:

```bash
pip install build
```

Then, from the root of your project (where `pyproject.toml` is located), run:

```bash
python -m build
```

This command will create a `dist/` directory containing:
-   `.whl` file (wheel): A built distribution, ready for installation.
-   `.tar.gz` file (sdist): A source distribution, containing your source code.

## 3. Uploading to PyPI

To upload your package, you'll typically use `twine`.

### Install `twine`

```bash
pip install twine
```

### Create a PyPI Account

If you don't have one, register at [pypi.org](https://pypi.org/). For testing, you can use [test.pypi.org](https://test.pypi.org/).

### Upload to TestPyPI (Recommended for first-timers)

It's highly recommended to upload to TestPyPI first to ensure everything works correctly without polluting the real PyPI.

```bash
twine upload --repository testpypi dist/*
```

Twine will prompt you for your TestPyPI username and password.

### Upload to PyPI

Once you're confident, upload to the real PyPI:

```bash
twine upload dist/*
```

Twine will prompt you for your PyPI username and password.

## 4. Installing Your Package

Once uploaded, anyone can install your package using `pip`:

```bash
pip install my-awesome-package
```

If you uploaded to TestPyPI, you need to specify the extra index URL:

```bash
pip install --index-url https://test.pypi.org/simple/ --no-deps my-awesome-package
```

## 5. Managing Dependencies

-   **`install_requires` (in `pyproject.toml` under `[project]`):** For core dependencies that your package needs to run.

    ```toml
    # pyproject.toml
    [project]
    # ...
    dependencies = [
        "requests>=2.28.1",
        "beautifulsoup4",
    ]
    ```

-   **`optional-dependencies`:** For dependencies that are only needed for specific features (e.g., `dev`, `docs`, `testing`). Users can install them like `pip install my-awesome-package[dev]`.

## 6. Versioning (Semantic Versioning)

It's good practice to follow Semantic Versioning (SemVer) for your package versions (e.g., `MAJOR.MINOR.PATCH`).

-   **MAJOR:** Incompatible API changes.
-   **MINOR:** Add functionality in a backward-compatible manner.
-   **PATCH:** Backward-compatible bug fixes.

## 7. `MANIFEST.in` (Including Non-Python Files)

If your package includes non-Python files (e.g., data files, configuration files, templates) that are not automatically picked up by `setuptools`, you need to specify them in a `MANIFEST.in` file in your project root.

```
# MANIFEST.in
include README.md LICENSE
recursive-include src/my_package_name/data *.json *.csv
```

## 8. `setup.py` (Legacy Configuration)

Historically, `setup.py` was the primary way to configure Python packages. While `pyproject.toml` is now preferred, you might still encounter `setup.py` in older projects.

```python
# setup.py (Legacy example)
from setuptools import setup, find_packages

setup(
    name='my-legacy-package',
    version='0.1.0',
    packages=find_packages(where='src'),
    package_dir={'': 'src'},
    install_requires=[
        'requests',
        'beautifulsoup4',
    ],
    # ... other metadata
)
```

For new projects, `pyproject.toml` is the recommended approach.
