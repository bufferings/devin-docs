---
layout: default
title: インストール方法
parent: はじめに
nav_order: 2
---

# インストール方法

ecspressoは複数の方法でインストールできます。

## Homebrew（macOSとLinux）

```console
$ brew install kayac/tap/ecspresso
```

## asdf（macOSとLinux）

```console
$ asdf plugin add ecspresso
# または
$ asdf plugin add ecspresso https://github.com/kayac/asdf-ecspresso.git

$ asdf install ecspresso 2.3.0
$ asdf global ecspresso 2.3.0
```

## aqua（macOSとLinux）

[aqua](https://aquaproj.github.io/)はCLIバージョンマネージャーです。

```console
$ aqua g -i kayac/ecspresso
```

## バイナリパッケージ

[Releases](https://github.com/kayac/ecspresso/releases)からダウンロードできます。

## CircleCI Orbs

```yaml
version: 2.1
orbs:
  ecspresso: fujiwara/ecspresso@2.0.4
jobs:
  install:
    steps:
      - checkout
      - ecspresso/install:
          version: v2.3.0 # or latest
          # version-file: .ecspresso-version
          os: linux # or windows or darwin
          arch: amd64 # or arm64
      - run:
          command: |
            ecspresso version
```

## GitHub Actions

```yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0 # or latest
          # version-file: .ecspresso-version
      - run: |
          ecspresso deploy --config ecspresso.yml
```
