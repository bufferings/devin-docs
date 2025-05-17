---
layout: default
title: よくあるユースケース
parent: クイックスタート
grand_parent: ecspresso
nav_order: 2
---

# よくあるユースケース

ここでは、ecspressoでよく行われる操作パターンを紹介します。

## Blue/Greenデプロイ

CodeDeployを使用したBlue/Greenデプロイを行います。

```console
# AppSpecの生成
$ ecspresso appspec > appspec.yaml

# デプロイの実行
$ ecspresso deploy
```

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant E as ecspresso
    participant CD as AWS CodeDeploy
    participant ECS as AWS ECS
    
    User->>E: appspecの生成
    E-->>User: appspec.yaml
    User->>E: deploy実行
    E->>CD: デプロイ開始
    CD->>ECS: タスク定義登録
    CD->>ECS: 新バージョンのタスク起動
    CD->>ECS: トラフィック転送
    CD->>ECS: 古いタスク停止
    CD-->>E: デプロイ完了
    E-->>User: 完了報告
```

## 環境変数を使った複数環境へのデプロイ

環境変数を使用して異なる環境に対応した設定を行います。

```console
# 開発環境へのデプロイ
$ ENV=dev ecspresso deploy --envfile .env.dev

# 本番環境へのデプロイ
$ ENV=prod ecspresso deploy --envfile .env.prod
```

## サービスのスケーリング

`scale`コマンドを使用してサービスのタスク数を調整します。

```console
# タスク数を5に設定
$ ecspresso scale --tasks=5

# Auto Scalingの設定
$ ecspresso deploy --suspend-auto-scaling=false --auto-scaling-min=2 --auto-scaling-max=10
```

## タスク実行時のオーバーライド

`run`コマンドでタスク実行時にコンテナ設定をオーバーライドします。

```console
$ ecspresso run --overrides='{"containerOverrides":[{"name":"app","command":["migrate"]}]}'
```

## サービス検証

`verify`コマンドを使用して、サービス設定の検証を行います。

```console
$ ecspresso verify
```

## リソース間の差分確認

`diff`コマンドで現在のECS設定との差分を確認します。

```console
$ ecspresso diff
```

## ロールバック戦略

```mermaid
graph TD
    A[問題検出] --> B{ロールバック判断}
    B -->|タスク定義のみ| C[ecspresso rollback --task-definition-only]
    B -->|サービス全体| D[ecspresso rollback]
    C --> E[検証]
    D --> E
    E -->|問題解決| F[完了]
    E -->|問題継続| G[さらに前のバージョンへ]
    G --> B
```
