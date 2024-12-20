---
layout: post
title:  "Setup Python environment on macOS"
date:   2024-12-13 15:35:51 +0800
categories: macOS Python
---
## Setup Python environment on macOS

First install [Homebrew](https://brew.sh/).

Then install [pyenv](https://github.com/pyenv/pyenv).

```shell
brew install pyenv
```

Install a specific version of Python.

```shell
pyenv install 3.12
```

Then install [uv](https://docs.astral.sh/uv/getting-started/).

```shell
brew install uv
```

Install uv-managed python even if there's already a Python installation on your system.
Once Python is installed, it will be used by `uv` command automatically.
When Python is installed by `uv`, it will not be available globally.

```shell
uv install 3.12.8
```

Created a new project.

```shell
uv init python-demos
```

Add a dependency.

```shell
uv add pytest
uv add pytest-cov

uv add pre-commit
```

Configure the `pre-commit` with tools upside.

`.pre-commit-config.yaml`
```yaml
repos:
- repo: https://github.com/psf/black
  rev: 24.10.0
  hooks:
  - id: black

- repo: https://github.com/PyCQA/isort
  rev: 5.13.2
  hooks:
  - id: isort

- repo: https://github.com/PyCQA/flake8
  rev: 7.1.1
  hooks:
  - id: flake8
    args: [
      "--ignore=E203,W503,E266",
      --max-line-length=120
    ]
```

Then install the git hooks.

```shell
uv run pre-commit install
```

Now when you run `git commit`, it will run those tools automatically.

## Run a file

Using [python-demos](https://github.com/PeteSong/demos/tree/main/python-demos) as an example.
```shell
cd python-demos

# check a file
source ./scripts/check.sh ./leetcode_solutions/lc2235.py

# run a file
uv run ./leetcode_solutions/lc2235.py

# run pytest
uv run pytest ./tests/test_leetcode_solutions/test_lc13.py
```
