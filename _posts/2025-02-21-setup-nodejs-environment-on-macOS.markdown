---
layout: post
title:  "Setup Node.js environment on macOS"
date:   2025-02-21 10:35:51 +0800
categories: macOS Node.js
---
## Setup Node.js environment on macOS

First install [Homebrew](https://brew.sh/).

Then install [fnm](https://github.com/Schniz/fnm).

```shell
brew install fnm
```

Install a specific version of Node.js.

```shell
fnm install 22
```

Then install [pnpm](https://pnpm.io/installation).

```shell
brew install pnpm

pnpm setup
```

Install packages globally

```shell
pnpm -g install neovim
pnpm -g install appium

# list the packages
pnpm -g list --dept=0
```

Check the versions

```shell
alias nodeversions="echo node: $(node -v); echo corepack: $(corepack -v); echo npm: $(npm -v); echo fnm: $(fnm --version); echo pnpm: $(pnpm -v)"

nodeversions
```

## Create a project

Created a [playwright](https://playwright.dev/docs/intro) project

```shell
cd ~/ws/github/demos/playwright-demos/
mkdir nodejs
cd nodejs
pnpm create playwright
```

## Run test in the project


```shell
pnpm exec playwright test
```

Open the test report

```shell
pnpm exec playwright show-report
```
