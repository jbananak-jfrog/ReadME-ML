---
title: Managing Dependencies
deprecated: false
hidden: false
metadata:
  robots: index
---
JFrog ML supports a variety of Python frameworks to manage model dependencies.

### Supported Python Versions

When building and managing your Python projects, different tools have varying levels of support for Python versions. Below is a summary of the supported Python versions for each tool:

* **Poetry** supports Python versions: 3.10 - 3.13
* **Conda** supports Python versions: 3.10 - 3.13
* **requirements.txt (pip)** supports Python versions: 3.10 - 3.13

### Using Poetry with JFrogML

<Callout icon="⚠️" theme="warning">
  **Warning**

  JFrogML uses **Poetry version 1.8.3**.JFrog ML supports `poetry.lock` files as long as they're under the same scope as the `pyproject.toml` file.
</Callout>

#### Model Directory Structure

```
frogml_based_model/
├── main/
├──── pyproject.toml
├──── poetry.lock
├── tests/
```

Both files `pyproject.toml` and `poetry.lock` will be used by Poetry while executing the `poetry install` command.

#### Example Project Setup

**pyproject.tom**l

```toml
[tool.poetry]
name = "example-project"
version = "0.1.0"
description = "Example project for production and development"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = ">=3.11,<3.12"
scipy = "^1.7"
scikit-learn = "^0.24"
catboost = "^1.0"

[tool.poetry.dev-dependencies]
frogml-cli = "*"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

> The `frogml-sdk` dependency is included only in the `dev` section, as it's needed for local development but not for remote builds. When you run the `frogml models build` command, the SDK version you used locally is automatically included in the remote environment.

### Using Conda with JFrogML

> JFrogML uses **Conda version 24.7.1**

#### Model Directory Structure

```
frogml_based_model/
├── main/
├──── conda.yml
├──── ...
├── tests/
```

The `conda.yml` file can be placed at the root level alongside `main` or within it—both structures work equally well.

#### Example Project Setup

To get started, here’s a basic `conda.yml` setup:

```yaml
name: example_conda_model
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.11
  - scipy
  - scikit-learn
  - catboost
  - pip:
      - # additional pip dependencies
```

<Callout icon="📘" theme="info">
  **Note**

  There’s no need to manually add `frogml-sdk` to the environment. JFrogML’s build process includes it automatically based on your local version.
</Callout>

### .frogmlignore file

Occasionally, you may want to exclude a file from the JFrog ML build but keep it in the repository with the model code. In such cases, add the `.frogmlignore` file to the root directory of our project.

In this file, we define the patterns to match files to exclude from the model build. For example, suppose we have the following file structure:

```yaml
.frogmlignore
main/
    __init__.py
    model.py
    README.md
tests/
    test_model.py
research/
    paper_a.pdf
    paper_b.pdf
```

If we want to exclude the entire `research` directory and the `README.md` file from the build, our `.frogmlignore` file may contain:

```
research
README.md
```

<Callout icon="📘" theme="info">
  **Note** - _**Hidden Files**_

  By default, JFrog ML disregards hidden files. Hidden files are files or directories whose names start with a dot (`.`) in Unix-like operating systems, or with the "Hidden" attribute set in Windows. These files are typically used to store configuration data or hold temporary information.

  For example, in the case that you have a directory with files and subdirectories, including a hidden file named `.config_file`. JFrog ML, following its default behavior, will exclude this file from processing when triggering a remote build.
</Callout>

### Incorporating Python Dependencies from .whl Files

JFrog ML facilitates the use of Python dependencies packaged as `.whl` files through `requirements.txt` and `conda.yaml` for managing dependencies. However, it is important to note that Poetry's support for dependencies from .whl files is limited.

#### 1. Preparing Your .whl Files

First, ensure your `.whl` file(s) are either uploaded with your model code or fetched from external storage. For instructions on uploading additional dependencies, refer to the FrogML CLI documentation (`frogml models build --help`). Below is an example directory structure for your model, where `main` is uploaded by default and the `dep` directory, containing the pandas dependency in a `.whl` file, is included via the `--dependency-required-folders dep` option in the FrogML command.

The wheel file has to be uploaded as part of an additional dependencies folder, and not as part of the main model folder.

Model Build Container Directory

```yaml
/frogml/model_dir/
.├── main                   # Main directory containing core code
│   ├── __init__.py        # An empty file that indicates this directory is a Python package
│   ├── model.py           # Defines the Credit Risk Model
│   └── conda.yaml         # Conda environment configuration
├── dep                    # Additional dependency directory added with --dependency-required-folders
│   └── pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
├── tests                  # Empty directory reserved for future test
│   └── ...                # Future tests
└── 
```

#### 2. Configuring Dependency Management Files

##### Conda

Include the `.whl` file in your `conda.yaml` as follows:

```yaml
name: test_model
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.11
  - pip:
    - "/frogml/model_dir/dep/pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
```

##### Poetry

pyproject.toml

```toml
[tool.poetry]
name = "example-project"
version = "0.1.0"
description = "Example project for production and development"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.11"
scipy = "^1.7"
scikit-learn = "^0.24"
catboost = "^1.0"
pandas = { path = "dep/pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" }

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

##### Requirements.txt

Directly reference the `.whl` file path relative to the requirements file location:

`requirements.txt`

```shell
## requirements file located in main model folder
./../deps/wheel_test-0.1-py3-none-any.whl

## requirements file located in model dir
./deps/wheel_test-0.1-py3-none-any.whl
```

#### 3. Using the Dependency in Your Code

Once the dependency is properly configured, you can import and use it in your Python code as usual:

`Python`

```python
import pandas as pd
```
