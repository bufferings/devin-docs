---
layout: default
title: インストール方法
parent: はじめに
grand_parent: ecspresso
nav_order: 2
---

# インストール方法

ecspressoは様々な方法でインストールできます。ここでは、各プラットフォームでのインストール方法を説明します。

## Homebrew（macOSとLinux）

macOSとLinuxでは、Homebrewを使用して簡単にインストールできます。

```bash
brew install kayac/tap/ecspresso
```

## asdf（macOSとLinux）

[asdf](https://asdf-vm.com/)はバージョン管理ツールで、ecspressoをインストールするために使用できます。

```bash
# プラグインの追加
asdf plugin add ecspresso
# または
asdf plugin add ecspresso https://github.com/kayac/asdf-ecspresso.git

# インストール
asdf install ecspresso 2.3.0
asdf global ecspresso 2.3.0
```

## aqua（macOSとLinux）

[aqua](https://aquaproj.github.io/)はCLIバージョン管理ツールで、ecspressoをインストールするために使用できます。

```bash
aqua g -i kayac/ecspresso
```

## バイナリパッケージ

各プラットフォーム用のバイナリパッケージは[GitHubのリリースページ](https://github.com/kayac/ecspresso/releases)からダウンロードできます。

1. リリースページから適切なバージョンとプラットフォーム用のバイナリをダウンロード
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

`version: latest`は各Orbバージョンで異なるバージョンのecspressoをインストールします：
- fujiwara/ecspresso@0.0.15
  - 最新のリリースバージョン（v2以降）
- fujiwara/ecspresso@1.0.0
  - v1.xの最新バージョン
- fujiwara/ecspresso@2.0.3
  - v2.xの最新バージョン

注意: 新しいバージョンのecspressoがリリースされると予期しない動作を引き起こす可能性があるため、`version: latest`の使用は推奨されません。

Orb `fujiwara/ecspresso@2.0.2`は`version-file: path/to/file`をサポートしており、ファイルで指定されたecspressoバージョンをインストールします。このバージョン番号には`v`プレフィックスはありません（例：`2.0.0`）。

## GitHub Actions

GitHub Actionsでecspressoを使用する場合は、Action `kayac/ecspresso@v2`を利用できます。このアクションはLinux(x86_64)用のecspressoバイナリを/usr/local/binにインストールします。

```yaml
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

ecspressoの最新バージョンを使用するには、パラメータ"latest"を渡します。

```yaml
      - uses: kayac/ecspresso@v2
        with:
          version: latest
```

`version: latest`は各Actionバージョンで異なるバージョンのecspressoをインストールします：
- kayac/ecspresso@v1
  - v1.xの最新バージョン
- kayac/ecspresso@v2
  - v2.xの最新バージョン

注意: 新しいバージョンのecspressoがリリースされると予期しない動作を引き起こす可能性があるため、`version: latest`の使用は推奨されません。

Action `kayac/ecspresso@v2`は`version-file: path/to/file`をサポートしており、ファイルで指定されたecspressoバージョンをインストールします。このバージョン番号には`v`プレフィックスはありません（例：`2.3.0`）。

## インストール確認

インストールが成功したかどうかを確認するには、以下のコマンドを実行します：

```bash
ecspresso version
```

バージョン情報が表示されれば、インストールは成功しています。
