---
title: Setup
---

## Setup

::: prereq

- Shell with Git version control tool installed and set up
- Ability to navigate filesystem and run commands from within a shell
- Ability to use basic Git commands from a shell
- Python version 3.8 or above installed (with `pip`and `venv` tools)
- Understanding of Python syntax to be able to read and follow code examples
- Understanding of managing Python dependencies and virtual environments (e.g. using `pip` and `venv` tools)
- `uv` Python package and dependency management tool installed
- [GitHub](https://github.com) account
- [Example code](https://github.com/softwaresaved/spacewalks_example) copied into your GitHub account

:::

### Shell with Git

On macOS and Linux, some version of a shell (e.g. `bash`) with Git will be available by default and no installation is needed.

If you do not have a bash shell installed on your system and require assistance with the installation, you can take a look at [the instructions provided by Software Carpentry](https://swcarpentry.github.io/python-novice-inflammation/#install-python)
for installing [shell](https://carpentries.github.io/workshop-template/install_instructions/#the-bash-shell) and [Git](https://carpentries.github.io/workshop-template/install_instructions/#git-1).

### Python

Python version 3.8 or above is required. Type `python -v` at your shell prompt and press enter to see what version of Python is installed on your system.
If you do not have Python installed on your system and require assistance with the installation, you can take a look at [the instructions provided by Software Carpentry](https://swcarpentry.github.io/python-novice-inflammation/#install-python)
for installing Python in preparation for undertaking their Python lesson.

### uv

You must install the Python package and dependency management tool `uv` (version 0.12 or higher) to follow along with the hands-on part of this lesson.
`uv` can be obtained from [Astral](https://docs.astral.sh/uv/).

You can check your version of `uv` with the `uv self version` command and update it, if needed, with `uv self update`.

### Code Editor

For the hands-on part of this topic you will need a code editor to navigate and edit software project files.
You can use, for example [Visual Studio Code (VS Code) - a widely used IDE](https://code.visualstudio.com/), or any other IDE or code editor of your choice.

### GitHub

We use [GitHub](https://github.com) to store code examples you will have to copy.
If you do not already have a GitHub account - you can open one for free.

### Example Code

[Example code](https://github.com/softwaresaved/spacewalks_example) we will use for the session can be obtained from GitHub.
Please make sure to copy this repository into your GitHub account by using the `Use this template` button on GitHub.

After creating your copy of the example code repository, using Git from a shell make a clone on your machine:

```bash
cd ~
git clone https://github.com/YOUR-GITHUB/spacewalks_example.git
cd spacewalks_example
```

