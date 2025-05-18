---
layout: default
title: Installation
parent: Introduction
grand_parent: ecspresso
nav_order: 2
---

# Installation

There are several ways to install ecspresso:

## Using Homebrew (macOS and Linux)

```shell
brew install kayac/tap/ecspresso
```

## Using GitHub Releases

Download the latest binary from [GitHub Releases](https://github.com/kayac/ecspresso/releases) and place it in your PATH.

```shell
# Example for Linux (amd64)
curl -LO https://github.com/kayac/ecspresso/releases/download/v2.x.x/ecspresso_2.x.x_linux_amd64.tar.gz
tar xzf ecspresso_2.x.x_linux_amd64.tar.gz
sudo install ecspresso /usr/local/bin
```

## Docker

```shell
docker run --rm -it \
  -v $HOME/.aws:/home/ecspresso/.aws:ro \
  -v $(pwd):/workdir \
  kayac/ecspresso:latest [commands...]
```

## GitHub Actions

ecspresso can be used in GitHub Actions workflows:

```yaml
- uses: kayac/ecspresso@v2
  with:
    version: v2.x.x
- run: ecspresso [commands...]
```

## Verifying Installation

After installation, verify that ecspresso is working correctly:

```shell
ecspresso version
```
