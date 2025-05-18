---
layout: default
title: インストール方法
parent: はじめに
nav_order: 2
---

# インストール方法

ecspressoは、Go言語で書かれたコマンドラインツールです。以下の方法でインストールすることができます。

## バイナリのダウンロード

GitHub Releasesページから、お使いのプラットフォーム向けのバイナリをダウンロードできます。

```bash
# 最新バージョンをダウンロード
curl -sL https://github.com/kayac/ecspresso/releases/latest/download/ecspresso-linux-amd64.zip > ecspresso.zip
unzip ecspresso.zip
chmod +x ecspresso
mv ecspresso /usr/local/bin/
```

## Homebrewを使用したインストール（macOS）

macOSをお使いの場合は、Homebrewを使用してインストールすることができます。

```bash
brew install kayac/tap/ecspresso
```

## Go言語を使用したインストール

Go言語の環境がある場合は、`go install`コマンドを使用してインストールすることもできます。

```bash
go install github.com/kayac/ecspresso/v2/cmd/ecspresso@latest
```

## Dockerを使用する方法

Dockerイメージも利用可能です。

```bash
docker pull ghcr.io/kayac/ecspresso:latest
docker run --rm -it -v $HOME/.aws:/root/.aws -v $(pwd):/work ghcr.io/kayac/ecspresso:latest [command]
```

## インストールの確認

インストールが完了したら、以下のコマンドでバージョンを確認できます。

```bash
ecspresso version
```

## AWS認証情報の設定

ecspressoはAWS SDKを使用してAWSリソースにアクセスします。以下のいずれかの方法でAWS認証情報を設定する必要があります。

1. AWS CLIの設定ファイル（`~/.aws/credentials`）
2. 環境変数（`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`）
3. EC2インスタンスプロファイル（EC2インスタンス上で実行する場合）
4. ECSタスクロール（ECSタスク内で実行する場合）

```bash
# 環境変数を使用する例
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_REGION=ap-northeast-1
```

## 次のステップ

インストールが完了したら、[基本的な使い方](../quickstart/basic-usage.html)を参照して、ecspressoの基本的な使い方を学びましょう。
