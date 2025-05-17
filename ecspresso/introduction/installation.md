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

macOSユーザーの場合、Homebrewを使用して簡単にインストールできます：

```console
$ brew install kayac/tap/ecspresso
```

## バイナリダウンロード

[GitHubリリースページ](https://github.com/kayac/ecspresso/releases)から、お使いのプラットフォーム向けの最新バイナリをダウンロードして、PATHの通ったディレクトリに配置します。

```console
# Linuxの場合の例
$ curl -L https://github.com/kayac/ecspresso/releases/download/vX.Y.Z/ecspresso_X.Y.Z_linux_amd64.tar.gz | tar xvz
$ sudo mv ecspresso /usr/local/bin/
```

## Docker

Dockerイメージも利用可能です：

```console
$ docker pull ghcr.io/kayac/ecspresso:latest
$ docker run --rm -it -v $HOME/.aws:/root/.aws -v $(pwd):/work ghcr.io/kayac/ecspresso:latest [command]
```

## GitHub Actions

GitHub Actionsでecspressoを使用する場合は、ワークフローファイルに以下のように記述します：

```yaml
steps:
  - uses: kayac/ecspresso@v2
    with:
      command: deploy
```

## CircleCI Orb

CircleCIでは、ecspresso orbを使用できます：

```yaml
orbs:
  ecspresso: kayac/ecspresso@1.0.0

jobs:
  deploy:
    executor: ecspresso/default
    steps:
      - checkout
      - ecspresso/install
      - ecspresso/deploy:
          config: config.yaml
```

## バージョン確認

インストール後、以下のコマンドでバージョンを確認できます：

```console
$ ecspresso version
```
