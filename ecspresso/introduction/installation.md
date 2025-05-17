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

[GitHub Releases](https://github.com/kayac/ecspresso/releases)から直接バイナリをダウンロードすることもできます。各プラットフォーム向けのバイナリが提供されています：

- Linux (amd64, arm64)
- macOS (amd64, arm64)
- Windows (amd64)

```console
# Linux (amd64)の場合
$ curl -Lo ecspresso.tar.gz https://github.com/kayac/ecspresso/releases/download/v2.5.0/ecspresso_2.5.0_linux_amd64.tar.gz
$ tar xzf ecspresso.tar.gz
$ chmod +x ecspresso
$ sudo mv ecspresso /usr/local/bin/

# macOS (arm64)の場合
$ curl -Lo ecspresso.tar.gz https://github.com/kayac/ecspresso/releases/download/v2.5.0/ecspresso_2.5.0_darwin_arm64.tar.gz
$ tar xzf ecspresso.tar.gz
$ chmod +x ecspresso
$ sudo mv ecspresso /usr/local/bin/
```

## Docker

Dockerイメージも利用可能です：

```console
$ docker pull ghcr.io/kayac/ecspresso:latest
$ docker run --rm -it -v $HOME/.aws:/root/.aws -v $(pwd):/work -w /work ghcr.io/kayac/ecspresso:latest [command]
```

## CI/CD環境での利用

### CircleCI Orbs

CircleCIで使用する場合、Orbsが利用可能です：

```yaml
orbs:
  ecspresso: fujiwara/ecspresso@2.0

jobs:
  deploy:
    executor: ecspresso/default
    steps:
      - checkout
      - ecspresso/install
      - run:
          name: Deploy
          command: |
            ecspresso deploy --config ecspresso.yml
```

### GitHub Actions

GitHub Actionsで使用する場合：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso-action@v2
        with:
          version: v2.5.0
      - run: ecspresso deploy
```

## バージョン確認

インストール後、以下のコマンドでバージョンを確認できます：

```console
$ ecspresso version
ecspresso v2.5.0
```

## 必要な権限

ecspressoを使用するには、AWS認証情報が必要です。以下の方法で認証情報を設定できます：

- AWS CLI設定ファイル（`~/.aws/credentials`）
- 環境変数（`AWS_ACCESS_KEY_ID`、`AWS_SECRET_ACCESS_KEY`）
- IAMロール（EC2インスタンスプロファイル、ECSタスクロールなど）

必要なIAM権限は、使用するecspressoのコマンドによって異なります。一般的に必要な権限には以下が含まれます：

- `ecs:*` - ECSリソースの管理
- `iam:PassRole` - タスク実行ロールとタスクロールの使用
- `cloudwatch:*` - ログの取得と監視
- `codedeploy:*` - Blue/Greenデプロイを使用する場合

詳細な権限設定については、各コマンドのドキュメントを参照してください。

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
