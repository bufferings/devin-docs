---
layout: default
title: インストール
nav_order: 2
parent: はじめに
grand_parent: ecspresso
---

# インストール

ecspressoは、複数の方法でインストールできます。

## バイナリダウンロード

[GitHubリリースページ](https://github.com/kayac/ecspresso/releases)から、最新のバイナリをダウンロードしてください。

Linux、macOS、Windowsのバイナリが利用可能です。ダウンロードしたバイナリを実行可能にして、PATHの通ったディレクトリに配置してください。

```bash
# Linux / macOS の場合
chmod +x ecspresso
mv ecspresso /usr/local/bin/
```

## Homebrew (macOS)

macOSでは、Homebrewを使ってインストールすることもできます。

```bash
brew install kayac/tap/ecspresso
```

## Docker

Dockerイメージも利用可能です。

```bash
docker pull ghcr.io/kayac/ecspresso:latest
docker run --rm -it -v $(pwd):/work -w /work ghcr.io/kayac/ecspresso:latest [command]
```

## バージョン確認

インストールが完了したら、以下のコマンドでバージョンを確認できます。

```bash
ecspresso version
```
