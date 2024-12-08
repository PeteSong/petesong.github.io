---
layout: default
title:  "Linux Tips"
date:   2024-12-08 18:55:51 +0800
categories: Linux
---

#### Homebrew
```shell
# List the packages that the user requests to install.
brew leaves

# List the packages that `pyenv` depends on.
brew deps --tree pyenv

# List the packages that depend on `tree-sitter`.
brew uses --recursive --install tree-sitter
```

#### MacPorts
```shell
# List the packages that the user requests to install.
port installed requested

# List the packages that `trash` depends on.
port rdeps trash

# List the packages that depend on `bzip2`.
port rdependents bzip2
```
