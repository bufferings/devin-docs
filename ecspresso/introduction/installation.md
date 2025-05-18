---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは複数の方法でインストールすることができます。

## バイナリリリース

[GitHubのリリースページ](https://github.com/kayac/ecspresso/releases)から、お使いのプラットフォーム用の最新のバイナリをダウンロードしてください。

```bash
# macOSの場合
$ curl -LO https://github.com/kayac/ecspresso/releases/download/v2.x.x/ecspresso_2.x.x_darwin_amd64.zip
$ unzip ecspresso_2.x.x_darwin_amd64.zip
$ mv ecspresso /usr/local/bin/

# Linuxの場合
$ curl -LO https://github.com/kayac/ecspresso/releases/download/v2.x.x/ecspresso_2.x.x_linux_amd64.tar.gz
$ tar xzf ecspresso_2.x.x_linux_amd64.tar.gz
$ mv ecspresso /usr/local/bin/
```

## Homebrewを使用する場合（macOS）

```bash
$ brew install kayac/tap/ecspresso
```

## Docker

Docker Imageも利用可能です。

```bash
$ docker pull ghcr.io/kayac/ecspresso:v2
$ docker run --rm -it -v $(pwd):/work -w /work ghcr.io/kayac/ecspresso:v2 --help
```

## インストールの確認

インストールが成功したことを確認するには：

```bash
$ ecspresso version
```

バージョン情報が表示されれば、インストールは成功しています。
