---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは、様々な方法でインストールできます。ここでは、各プラットフォームでのインストール方法を説明します。

## Homebrew（macOSとLinux）

macOSとLinuxでは、Homebrewを使用して簡単にインストールできます。

```console
$ brew install kayac/tap/ecspresso
```

## asdf（macOSとLinux）

[asdf](https://asdf-vm.com/)は、複数のランタイムバージョンを管理するためのツールです。

```console
$ asdf plugin add ecspresso
# または
$ asdf plugin add ecspresso https://github.com/kayac/asdf-ecspresso.git

$ asdf install ecspresso 2.3.0
$ asdf global ecspresso 2.3.0
```

## aqua（macOSとLinux）

[aqua](https://aquaproj.github.io/)は、CLIバージョン管理ツールです。

```console
$ aqua g -i kayac/ecspresso
```

## バイナリパッケージ

[GitHubのリリースページ](https://github.com/kayac/ecspresso/releases)から、各プラットフォーム向けのバイナリパッケージをダウンロードできます。

1. リリースページから、お使いのプラットフォーム（Linux、macOS、Windows）に合ったバイナリをダウンロード
2. ダウンロードしたファイルを解凍
3. 実行可能ファイルをPATHの通ったディレクトリに配置

## CircleCI Orbs

CircleCIでecspressoを使用する場合は、Orbsを利用できます。

```yaml
version: 2.1
orbs:
  ecspresso: fujiwara/ecspresso@2.0.4
jobs:
  install:
    steps:
      - checkout
      - ecspresso/install:
          version: v2.3.0 # または latest
          # version-file: .ecspresso-version
          os: linux # または windows または darwin
          arch: amd64 # または arm64
      - run:
          command: |
            ecspresso version
```

`version: latest`を指定すると、Orbのバージョンに応じて異なるバージョンのecspressoがインストールされます。
- fujiwara/ecspresso@0.0.15
  - 最新のリリースバージョン（v2以降）
- fujiwara/ecspresso@1.0.0
  - v1.xの最新バージョン
- fujiwara/ecspresso@2.0.3
  - v2.xの最新バージョン

注意: `version: latest`は、新しいバージョンのecspressoがリリースされたときに予期しない動作を引き起こす可能性があるため、推奨されません。

Orb `fujiwara/ecspresso@2.0.2`以降は、`version-file: path/to/file`をサポートしており、ファイルで指定されたecspressoのバージョンをインストールします。このバージョン番号には`v`プレフィックスはありません（例：`2.0.0`）。

## GitHub Actions

GitHub Actionsでecspressoを使用する場合は、Action `kayac/ecspresso@v2`を利用できます。このアクションは、Linux(x86_64)用のecspressoバイナリを/usr/local/binにインストールします。

```yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0 # または latest
          # version-file: .ecspresso-version
      - run: |
          ecspresso deploy --config ecspresso.yml
```

最新バージョンのecspressoを使用するには、パラメータ「latest」を渡します。

```yaml
      - uses: kayac/ecspresso@v2
        with:
          version: latest
```

`version: latest`を指定すると、Actionのバージョンに応じて異なるバージョンのecspressoがインストールされます。
- kayac/ecspresso@v1
  - v1.xの最新バージョン
- kayac/ecspresso@v2
  - v2.xの最新バージョン

注意: `version: latest`は、新しいバージョンのecspressoがリリースされたときに予期しない動作を引き起こす可能性があるため、推奨されません。

Action `kayac/ecspresso@v2`は、`version-file: path/to/file`をサポートしており、ファイルで指定されたecspressoのバージョンをインストールします。このバージョン番号には`v`プレフィックスはありません（例：`2.3.0`）。

## バージョン確認

インストール後、以下のコマンドでecspressoのバージョンを確認できます。

```console
$ ecspresso version
ecspresso 2.3.0
```

## 次のステップ

- [基本的な使い方](../quickstart/basic-usage.html)を参照して、ecspressoの基本的な使い方を学びましょう
