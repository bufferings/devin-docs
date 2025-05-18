---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは、複数の方法でインストールできます。

## バイナリのダウンロード

最も簡単な方法は、GitHub Releasesから事前にビルドされたバイナリをダウンロードすることです。

```bash
# 最新リリースをダウンロード（Linux/amd64の例）
curl -sL https://github.com/kayac/ecspresso/releases/download/latest/ecspresso_linux_amd64.tar.gz | tar xzC /tmp
sudo install /tmp/ecspresso /usr/local/bin/ecspresso
```

macOSユーザーの場合:

```bash
curl -sL https://github.com/kayac/ecspresso/releases/download/latest/ecspresso_darwin_amd64.tar.gz | tar xzC /tmp
sudo install /tmp/ecspresso /usr/local/bin/ecspresso
```

## パッケージマネージャーを使用する

### aquaを使用する場合

```bash
aqua g -i kayac/ecspresso
```

### asdfを使用する場合

```bash
asdf plugin add ecspresso
asdf install ecspresso latest
asdf global ecspresso latest
```

## ソースからビルドする

Goがインストールされている場合は、ソースからビルドすることもできます：

```bash
git clone https://github.com/kayac/ecspresso.git
cd ecspresso
make install
```

## インストールの確認

インストールが成功したことを確認するには、以下のコマンドを実行します：

```bash
ecspresso version
```

バージョン情報が表示されれば、インストールは成功しています。
