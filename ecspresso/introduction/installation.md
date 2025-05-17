---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは複数の方法でインストールできます。

## Homebrew（macOS）

macOSユーザーは、Homebrewを使用して簡単にインストールできます。

```console
$ brew install kayac/tap/ecspresso
```

## バイナリのダウンロード

[GitHub Releases](https://github.com/kayac/ecspresso/releases)から直接バイナリをダウンロードすることもできます。

```console
$ curl -LO https://github.com/kayac/ecspresso/releases/download/v2.X.X/ecspresso_v2.X.X_linux_amd64.tar.gz
$ tar xzf ecspresso_v2.X.X_linux_amd64.tar.gz
$ mv ecspresso /usr/local/bin/
```

※ バージョンとプラットフォームに応じて適切なファイル名を指定してください。

## Dockerイメージ

Dockerイメージも利用可能です。

```console
$ docker pull kayac/ecspresso:latest
$ docker run --rm -it -v $PWD:/work -w /work kayac/ecspresso:latest [コマンド]
```

## 動作確認

インストール後、以下のコマンドでバージョンを確認できます。

```console
$ ecspresso version
```
