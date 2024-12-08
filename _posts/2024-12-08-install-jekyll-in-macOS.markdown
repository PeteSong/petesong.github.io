---
layout: post
title:  "Install Jekyll in macOS"
date:   2024-12-08 18:55:51 +0800
categories: jekyll
---
Followed the [page](https://jekyllrb.com/docs/installation/macos/) to install Jekyll on macOS.

Hardware: MBP M3
OS: macOS Sonoma 14.7.1

```shell
brew install chruby ruby-install
ruby-install ruby 3.3.6

echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc

## run 'chruby' to see actual version
## use `.ruby-version` file under the repository to auto-switch ruby version
echo "ruby-3.3.6" >> ./.ruby-version
```

Open another terminal to activate the specific ruby version

```shell
## check ruby and gem is active and with right version
ruby --version && gem --version

## install jekyll
gem install jekyll
```

Since did not find the way to init the current folder by jekyll,
so use jekyll to new a project, then copy all the stuff into the project `petesong.github.io`.

```shell
cd petesong.github.io
jekyll new newblog
mv ./newblog/* ./
rm -rf newblog

## run the server to check the site in browser
jekyll server
```
