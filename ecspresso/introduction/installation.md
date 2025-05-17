---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは複数の方法でインストールできます。

## Homebrew

macOSユーザーの場合、Homebrewを使用してインストールできます。

```console
$ brew install kayac/tap/ecspresso
```

## asdf

asdfプラグインを使用してインストールする場合：

```console
$ asdf plugin add ecspresso
$ asdf install ecspresso latest
$ asdf global ecspresso latest
```

## aqua

aquaを使用してインストールする場合：

```console
$ aqua g -i kayac/ecspresso
```

## GitHub Releases

[GitHub Releases](https://github.com/kayac/ecspresso/releases)から直接バイナリをダウンロードすることもできます。

## CircleCI Orbs

CircleCIで使用する場合、Orbsが利用可能です：

```yaml
orbs:
  ecspresso: fujiwara/ecspresso@2.0
```

## GitHub Actions

GitHub Actionsで使用する場合：

```yaml
- uses: kayac/ecspresso-action@v2
```
