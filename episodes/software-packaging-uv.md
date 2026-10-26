---
title: "Software packaging with uv"
teaching: 15
exercises: 45
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do I package a Python project with `uv`?
- What project files does `uv` create/maintain and what are they used for?
- What is `pyproject.toml` and why has it become the standard project configuration file?
- How does `uv` manage dependencies and virtual environments?
- What is a lock file and why is it important for reproducible software?
- How do I add, remove and update project dependencies?
- How can I run Python scripts using `uv`?
- How do I build a source distribution and a wheel using `uv`?
- How can I migrate an existing project from `pip` and `venv` to `uv`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain the role of `uv` in modern Python project development.
- Migrate an existing pip/venv-based projects to a `uv` development workflow.
- Manage project dependencies and understand the purpose of lock files.
- Describe the structure and role of `pyproject.toml` as the central project configuration file.
- Run Python commands and scripts using `uv` without manually managing virtual environments.
- Build Python source distributions and wheel packages for distribution.

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

`uv` is a multi-functional tool, that performs a number of tasks:

- installs and manages Python versions,
- installs and runs Python tools (Python packages which provide applications that can be used as command line tools) published as Python packages (e.g. `ruff`, `pip`, etc.),
- provides comprehensive Python project and dependency management capabilities and handles virtual environments with a universal lock file,
- runs Python scripts,
- includes a pip-compatible interface for users with existing pip-based development workflows,
- is really performant and much faster than `pip`.

The core `uv` software project development workflow looks like as follows:

1. project creation and setup - `uv` can create a new project layout for you so you start with a clean structure instead of a blank folder
2. virtual environments - `uv` manages virtual environments directly, which means you do not need to switch between different tools to create and activate environments
3. dependency installation - `uv` installs packages quickly while still supporting the same dependency ecosystem you may already be familiar with
4. running commands inside the environment - `uv` can run Python commands or scripts inside the environment without you having to manually activate it
5. (optionally) package and publish your code.

### `uv` - key files to know

`uv` uses a combination of the following files and directories in your project (which it keeps in sync with one another) to do its job:

- `pyptoject.toml` file - to maintain the project's metadata and configuration
- `.venv` folder - to maintain the development environment
- `uv.lock` file - to record (lock) the development environment.

### `uv` project commands

There is a number of `uv` commands to handle Python projects as part of the development workflow (which manipulate and sync the files and directories above); we give a short summary below.

::: callout
### `uv` project commands

A summary of `uv` commands for creating and working on Python projects, i.e., with a `pyproject.toml` file.

- `uv init`: create a new Python project (and start a new `pyptoject.toml` file).
- `uv add`: add a dependency to the project (locking and syncing the environment).
- `uv remove`: remove a dependency from the project (locking and syncing the environment).
- `uv sync`: sync the project's dependencies with the environment (i.e. into the `.venv` folder in the project directory).
- `uv lock`: create a lock file for the project's dependencies (i.e. "locking" the current environment).
- `uv run`: run a Python command or a Python script in the project environment (automatically locking and syncing if anything is out-of-date).
- `uv tree`: view the dependency tree for the project.
- `uv build`: build the project into distribution archives.
- `uv publish`: publish the project to a package index.

Project commands are a subset of [all `uv` commands](https://docs.astral.sh/uv/getting-started/features/).

Check out [the `uv` guides](https://docs.astral.sh/uv/guides/) covering the full set of `uv` features.

:::

## Initialising the package with `uv`

Let's go to our example spacewalks code.

This is an existing project where you currently use `venv` and `pip` tools to manage dependencies and environments.
You want to migrate to using `uv` for your development workflow.

If you have not done it already, make a copy of [example project repository](https://github.com/softwaresaved/spacewalks_example) into your GitHub account by using the `Use this template` button on GitHub.
Next, clone your copy of the repository locally:

```bash
$ cd ~
$ git clone https://github.com/YOUR-GITHUB/spacewalks_example
```

Let's navigate into the `spacewalks_example` project directory and examine it a bit.

```bash
$ cd spacewalks_example
$ ls -la
total 56
drwxr-xr-x   15 mbassan2  staff    480 23 Jul 17:21 .
drwx------@ 897 mbassan2  staff  28704 23 Jul 17:21 ..
drwxr-xr-x   12 mbassan2  staff    384 23 Jul 17:21 .git
-rw-r--r--    1 mbassan2  staff     26 23 Jul 17:21 .gitignore
-rw-r--r--    1 mbassan2  staff    721 23 Jul 17:21 CITATION.cff
drwxr-xr-x    3 mbassan2  staff     96 23 Jul 17:21 data
drwxr-xr-x    7 mbassan2  staff    224 23 Jul 17:21 docs
-rw-r--r--    1 mbassan2  staff   3991 23 Jul 17:21 eva_data_analysis.py
-rw-r--r--    1 mbassan2  staff   1090 23 Jul 17:21 LICENSE
-rw-r--r--    1 mbassan2  staff    296 23 Jul 17:21 mkdocs.yml
-rw-r--r--    1 mbassan2  staff   2009 23 Jul 17:21 README.md
-rw-r--r--    1 mbassan2  staff    805 23 Jul 17:21 requirements.txt
drwxr-xr-x    5 mbassan2  staff    160 23 Jul 17:21 results
drwxr-xr-x   12 mbassan2  staff    384 23 Jul 17:21 site
drwxr-xr-x    3 mbassan2  staff     96 23 Jul 17:21 tests
```

We can see the `requirements.txt` file used to record dependencies from the development environment.

To migrate from the existing development workflow to use `uv`, you need to use `uv init` first to initiate the project and get the basic `pyptoject.toml` file created.

From within project root, do:

```bash
$ uv init
```

::: callout
### Using `uv` for new projects

If you are starting a brand new Python project then you can use `uv` straight away to replace all the other tools you may have used in the past in your development workflow.

For example, to create a new Python project called "hello-world" using the `uv init` command, do:

```bash
$ uv init hello-world
$ cd hello-world
```

`uv` will create the following files and directories as a basic scaffolding for your Python project:

```text
.
├── .git/
├── src/
│   └── hello-world/
│       └── __init__.py
├── .gitignore
├── .python-version
├── README.md
└── pyproject.toml
```
:::

Running `uv init` in our existing project has added the following files and directories:

```text
.
├── src/
│   └── spacewalks/
│       └── __init__.py
├── .python-version
└── pyproject.toml
```

The `__init__.py` Python file contains a simple "Hello world" program.

The `.python-version` file contains the project's default Python version (based on what is installed on your machine). 
This file tells `uv` which Python version to use when creating the project's virtual environment.

`uv` has also created a starter `pyproject.toml` file to store the metadata about your project.

```toml
[project]
name = "spacewalks"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

### `pyproject.toml` structure

`pyproject.toml` is a configuration file introduced by [PEP 518](https://peps.python.org/pep-0518/) to standardise Python project setup. 
Written in TOML (Tom’s Obvious, Minimal Language), it serves as a single source of truth for your project’s build system, metadata, and tool configurations. 
It tells tools like `uv`, `pip`, `poetry`, or `hatch` how to construct your project.

Unlike the older `setup.py` approach, which contains executable Python code, `pyproject.toml` uses a declarative, static configuration format. 

`pyptoject.toml` structure contains three main components (tables):

- **\[build-system]** table: defines the tools and other dependencies are needed to build your project's source distribution and wheel (built distribution).
- **\[project]** table: stores metadata like your project’s name, version, and dependencies.
- **\[tool]** table: configures settings for development tools like linters or formatters.

The [build-system] table is not compulsory - you can use `pyproject.toml` just to store metadata and configuration. 
If your project is an application (i.e. not designed to be published as a package), you do not need a [build-system] table. 
`uv` will install and manage your dependencies, but it will not attempt to build the project itself.
If you are building a package or library to be published, the [build-system] table is required so `uv` knows which build backend to use (e.g. `uv_build`, `setuptools`, `hatch`).

For example:

```toml
[build-system]
requires = ["uv_build>=0.11.31,<0.12"]
build-backend = "uv_build"
```

Or:
```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```

`uv` supports various build backends as specified by [PEP 517](https://peps.python.org/pep-0517/), but also provides a native build backend `uv_build` that integrates tightly with `uv` to improve performance and user experience.

When you create a new project with `uv init`, the `uv_build` backend will be used by default if you issue the `uv build` command to create the source distribution and wheel for your project.
`uv_build` backend has reasonable defaults and requires zero configuration for most users so you do not actually have to declare it in `pyptoject.toml` unless you want to change it to another build backend or configure it further.

You can initialise projects as packages even even you are just developing applications by running `uv init --package` - this will setup the project using a src-layout structure and setup `uv_build` as a build system in your `pyptoject.toml`.

The [tool] table has tool-specific subtables, e.g., [tool.uv], [tool.hatch], content of which are defined by each tool. 
You would need to consult the particular tool’s documentation to know what it can contain.

A minimal `pyproject.toml` just requires the [project] table.

::: callout
### `pyproject.toml` guide

`pyproject.toml` file can be used to declare many more configuration parameters about Python projects that we are not going into details here.
A [good guide on writing `pyproject.toml` files](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/) can be found from [Python Packaging User Guides website](https://packaging.python.org/).

:::

## Installing the package

A common convention is to keep Python code in a directory called `src` under the project root directory.
The directory `spacewalks` represents the top-level package.
The presence of an `__init__.py` file within `spacewalks` registers that directory as a package.
The file `__init__.py` is run when the package is imported with `import spacewalks`.

Let us see this in action.
Launch a Python console within the virtual environment.

```bash
# Activate virtual environment
source .venv/bin/activate     # Linux and macOS
source .venv/Scripts/activate # Windows

# Launch Python console
python
```

Within the Python console, import the `main` function from `spacewalks`.

```python
from spacewalks import main
```

This should fail with the following output:

```output
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    from spacewalks import main
ModuleNotFoundError: No module named 'spacewalks'
```

This failed because our package is not installed in our virtual environment.

Exit the Python console with the `exit()` command and install our package with `pip`.

```bash
pip install .
```

Now relaunch the Python console.
Within the Python console, import the `main` function from `spacewalks` and execute it.

```python
from spacewalks import main
main()
exit()
```

This should output:

```output
Hello from spacewalks!
```

### Project scripts

By default, `uv` creates an entry in the `project.scripts` section of `pyproject.toml`.

```toml
[project.scripts]
spacewalks = "spacewalks:main"
```

This defines a command which gets installed in your environment when you install the package.
Here, the command is `spacewalks` which invokes the `main` function in the `spacewalks` package namespace.
Instead of launching the Python console, importing `main` from `spacewalks`, and running it, we can execute it with the following shell command:

```bash
spacewalks
```

This should output `Hello from spacewalks!` as before.

## Running commands with `uv`

The `uv run` command can be used to run Python scripts or Python commands in your project environment.
It precedes commands to be run with the latest build of your project and ensures the virtual environment contains the correct versions of project dependencies.

To see this in action, first delete the virtual environment we made earlier.

```bash
deactivate
rm -r .venv
```

Now, if we try to run the `spacewalks` command we get an error saying the command is not found.
Instead, run `spacewalks` using the `uv run` command:

```bash
uv run spacewalks
```

`uv` creates a virtual environment (`.venv/`) and a lock file (`uv.lock`) the first time you run any project command, i.e., `uv add`, `uv run`, `uv sync`, or `uv lock`.
The lock file contains details of all packages and their versions required by the project.

During every `uv run` invocation, `uv` will verify that the lock file is up-to-date with the `pyproject.toml`, and that the environment is up-to-date with the lock file, keeping your project in-sync without the need for manual intervention.
`uv run` guarantees that your command is run in an environment with all required dependencies at their locked versions.

The project structure now looks like the following - with new parts added that all work together and allow `uv` to manage your project.

```text
.
├── .git/
├── .venv/
├── data/
├── docs/
├── results/
├── site/
├── src/
├── .gitignore
├── .python-version
├── README.md
├── eva_data_analysis.py
├── pyproject.toml
└── uv.lock
```

The command `uv run` can also be used in place of the regular `python` command when running scripts.
For example, to run the data analysis script:

```bash
uv run eva_data_analysis.py
```

However, we get the following import error:

```output
ModuleNotFoundError: No module named 'matplotlib'
```

This is because `uv` uses the `pyproject.toml` file to create the virtual environment and install our package.
This file should contain a list of dependencies required for our code to run.

::: callout
### Manually updating the virtual environment

Alternatively, you can use `uv sync` to manually update `.venv` then activate it before executing a Python command or running your Python script:

```bash
$ uv sync
$ source .venv/bin/activate # macOS or Linux
$ source .venv/Scripts/activate # Windows
$ spacewalks
```

Note: if you want to run scripts and commands using the Python command (i.e. without `uv run`) - the virtual environment must be active.
:::

## Managing dependencies with `uv`

Next, we need to install dependencies for our project, which are recorded in `requirements.txt` file.
By looking at the code in `eva_data_analysis.py` Python script, we can tell we require `matplotlib` and `pandas` packages.

We can use the `uv add` command to install them:

```bash
$ uv add matplotlib pandas
```

Alternatively, we can use the `uv add` command with the `-r` flag to add all dependencies from the `requirements.txt` file in a single go.

```bash
$ uv add -r requirements.txt
```

You can also specify version constraints or alternative sources for packages with the `uv add` command.
For example, to add a particular version of the [Requests](https://requests.readthedocs.io/en/latest/) package:

```bash
$ uv add 'requests==2.31.0'
```

Or, to add a dependency sourced from GitHub (as opposed to the default PyPI):

```bash
$ uv add git+https://github.com/psf/requests
```

To remove a dependency, you can use `uv remove`:

```bash
$ uv remove requests
```

This will also remove any other packages which Requests depended on, provided other project dependencies do not need them.

Now we should be able to run our script.

```bash
uv run eva_data_analysis.py
```

### Runtime and development dependencies

Tools like `uv` understand that there are two different types of dependencies: runtime dependencies and development dependencies.
Runtime dependencies need to be installed for our code to run, like `matplotlib` for our code.
Development dependencies are an essential part of the development process for a project, but are not required to run it.
For example, `mkdocs` used to build the documentation for our software project or `pytest` to run tests.

With `uv add matplotlib` and `uv add -r requirements.txt` commands, `uv` will add all packages to the list of runtime dependencies in `pyproject.toml` (regardless if they are runtime or development dependencies - there is not way to tell the difference with `requirements.txt`).

To add `mkdocs` and `pytest` as development dependencies, the `--group` option can be used:

```bash
$ uv add --group dev mkdocs pytest
```

This will add a new section to the `pyproject.toml` file for development dependencies, e.g.

```toml
[dependency-groups]
dev = [
    "mkdocs>=1.6.1",
    "pytest>=9.1.1",
]
```

`dev` is treated as a special kind of group by `uv`.
By default, `uv` will install all `dev` group dependencies alongside main package dependencies when you run `uv` commands.
As a special group, the option `--dev` can be used instead of `--group dev`.

The `--group` option can be used to define other non-runtime dependency groups.
For example, you might have an `benchmark` group which contains dependencies required to run benchmarks for your software.

Let's run the tests for our project, replacing `python` with `uv run`:

```bash
uv run -m pytest tests/
```

When someone else installs our package on their machine using `pip`, only the runtime dependencies will be installed since development dependencies are not needed to run the code.

At this point, you can delete `requirements.txt` and carry on running your Python scripts or Python tools with `uv`.
Recall that if you are using `uv` to run Python commands - you do not have to activate the environment explicitly.
If you prefer to use Python directly, you will have to activate your environment first but switch to `uv` for managing it from whatever tools you used before.

## Adding modules to our package

Python files within `spacewalks` are called submodules of the package.
For example, a submodule called `voskhod.py` (named after the space mission where Alexei Leonov took the world's first spacewalks) could be imported using the namespace `spacewalks.voskhod`.

Currently, our `eva_data_analysis.py` code is not a part of our `spacewalks` package.
We can include our `eva_data_analysis.py` code as a module in our new `spacewalks` package.
Move the file to the `src/spacewalks` directory give it a more concise name.

```bash
mv eva_data_analysis.py src/spacewalks/analyse.py
```

Now, we can import functions from the analysis code.
Launch the Python console and import the `read_json_to_dataframe` function from the `analyse` module.

```bash
uv run python
```

In the Python console, run:

```python
from spacewalks.analyse import read_json_to_dataframe
help(read_json_to_dataframe)  # Displays help message (quit window with 'q')
exit()
```

Python allows you to run installed modules as scripts with the `-m` option followed by the module namespace (e.g. `spacewalks.analyse`).
Since `uv run` can be used in place of the `python` command, we can run the script with the following command:

```bash
uv run -m spacewalks.analyse
```

However, if we now try to run the tests:

```bash
uv run -m pytest tests/
```

They fail with the following output:

```output
================================================ ERRORS ================================================
_____________________________ ERROR collecting tests/test_eva_analysis.py ______________________________
ImportError while importing test module '/Users/a.j.lyttle@bham.ac.uk/Repos/github.com/alexlyttle/spacewalks/tests/test_eva_analysis.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
/opt/homebrew/Cellar/python@3.14/3.14.10/Frameworks/Python.framework/Versions/3.14/lib/python3.14/importlib/__init__.py:88: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
tests/test_eva_analysis.py:2: in <module>
    from eva_data_analysis import (
E   ModuleNotFoundError: No module named 'eva_data_analysis'
======================================= short test summary info ========================================
ERROR tests/test_eva_analysis.py
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
=========================================== 1 error in 0.10s ===========================================
```

This is because the module they were importing from has moved.

:::: challenge
### Update tests

Go to `tests/test_eva_analysis.py` and update the import statements to import from our new `analysis` module.
Confirm that the tests run after making your changes.

::: solution

The top of `tests/test_eva_analysis.py` should now look like this:

```python
import pytest
from spacewalks.analyse import (
    text_to_duration,
    calculate_crew_size
)
```

:::
::::

<!-- TODO: add challenge or callout to include the CLI as the main project script -->

## Building distributions

`uv build` command can be used to build source distributions and binary distributions (wheels) for your project.

Once we have made sure our project is in the right structure, we can go ahead and build a distributable version of our software as before:

```bash
$ uv build
$ ls dist/
spacewalks_example-0.1.0-py3-none-any.whl	
spacewalks_example-0.1.0.tar.gz
```

The one we care most about is the `.whl` or wheel file. 
This is the file that package managers use to distribute and install Python packages, so this is the file we would need to share with other people who want to install our software.

By convention distributable package names use hyphens, whereas module package names use underscores. 
While we could choose to use underscores in a distributable package name, we cannot use hyphens in a module package name, as Python will interpret them as a minus sign in our code when we try to import them.

Now if we gave this wheel file to someone else, they could install it using `uv` (or `pip` or another package installer).

```bash
$ python3 -m pip install spacewalks_example-0.1.0-py3-none-any.whl
```

And then they could import our package in their own Python environment:

```python
import spacewalks
```

They can also run the spacewalks command:

```bash
spacewalks
```

Or the analysis module with:

```bash
python -m spacewalks.analyse
```

::: callout
### Managing the package version

After we have been working on our code for a while and want to publish an update, we just need to update the version number in the pyproject.toml file (using SemVer perhaps), then use `uv` to build and publish the new version.

`uv` can help easily increment the version number following Semantic Versioning conventions. For example, to increment the minor version number, we can do:

```bash
$ uv version --bump minor
```

Then the version number in `pyproject.toml` will be updated from 0.1.0 to 0.2.0.
:::

## Summary

`uv` provides a modern, standards-based workflow that simplifies Python project management by bringing dependency management, virtual environments, packaging and publishing together in a single tool.

::: keypoints

- `uv` combines Python installation, dependency management, virtual environments, packaging and publishing into a single, high-performance tool.
- `pyproject.toml` is the standard configuration file for modern Python projects and stores project metadata, dependencies and tool configuration.
- Dependencies are declared in `pyproject.toml`, while `uv.lock` records the exact package versions needed to reproduce the project environment with `uv`.
- `uv` automatically keeps the project environment and lock file synchronised, reducing manual environment management.
- `uv` run executes commands in the correct project environment without requiring manual activation of a virtual environment.
- Python packages are typically distributed as source distributions (.tar.gz) and wheels (.whl), which can be built using `uv` build.
- Existing `pip` and `venv` development workflows can be migrated to `uv` with only a few commands for most projects.

:::
