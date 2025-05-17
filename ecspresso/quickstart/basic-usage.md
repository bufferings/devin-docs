---
layout: default
title: 基本的な使い方
parent: クイックスタート
grand_parent: ecspresso
nav_order: 1
---

# 基本的な使い方

ecspressoを使えば、既存のECSサービスを簡単にコードで管理できます。

## 初期設定

既存のECSサービスから始める場合は、`ecspresso init`コマンドを使用します：

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
2019/10/12 01:31:48 myservice/default save service definition to ecs-service-def.json
2019/10/12 01:31:48 myservice/default save task definition to ecs-task-def.json
2019/10/12 01:31:48 myservice/default save config to ecspresso.yml
```

生成されたファイルを確認してください：
- `ecspresso.yml` - 設定ファイル
- `ecs-service-def.json` - サービス定義
- `ecs-task-def.json` - タスク定義

これで、ecspressoを使ってサービスをデプロイできるようになりました！

```console
$ ecspresso deploy --config ecspresso.yml
```

## テンプレート機能の活用

ecspressoはサービスとタスク定義ファイルをテンプレートとして読み込むことができます。
典型的な使用例として、タスク定義ファイル内のイメージタグを置き換えることができます。

ecs-task-def.jsonを以下のように変更します：

```diff
-  "image": "nginx:latest",
+  "image": "nginx:{{ must_env `IMAGE_TAG` }}",
```

環境変数`IMAGE_TAG`を使ってサービスをデプロイします：

```console
$ IMAGE_TAG=stable ecspresso deploy --config ecspresso.yml
```

## 設定ファイルの構造

ecspressoの設定ファイル（YAML、JSON、またはJsonnet形式）の例：

```yaml
region: ap-northeast-1 # または AWS_REGION 環境変数
cluster: default
service: myservice
task_definition: taskdef.json
timeout: 5m # デフォルトは10m
ignore:
  tags:
    - ecspresso:ignore # サービスとタスク定義のタグを無視
```

## デプロイの流れ

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Ecspresso as ecspresso
    participant ECS as Amazon ECS
    User->>Ecspresso: ecspresso deploy
    Ecspresso->>Ecspresso: タスク定義ファイルを読み込み
    Ecspresso->>Ecspresso: テンプレート変数を置換
    Ecspresso->>ECS: 新しいタスク定義を登録
    ECS-->>Ecspresso: タスク定義ARN
    Ecspresso->>ECS: サービスを更新
    Ecspresso->>Ecspresso: サービスが安定するまで待機
    Ecspresso-->>User: デプロイ完了
```

ecspressoを使用するための基本的な手順を説明します。

## 初期化

まず、ecspressoの設定ファイルを作成します。v2では、YAML形式とJsonnet形式の両方がサポートされています。

```console
$ ecspresso init --region ap-northeast-1 --cluster your-cluster-name --service your-service-name
```

Jsonnet形式で初期化する場合：

```console
$ ecspresso init --region ap-northeast-1 --cluster your-cluster-name --service your-service-name --jsonnet
```

このコマンドは、現在のディレクトリに設定ファイル（`ecspresso.yml`または`ecspresso.jsonnet`）、`ecs-service-def.json`、`ecs-task-def.json`を作成します。

## 設定ファイル

`ecspresso.yml`は以下のような内容になります：

```yaml
region: ap-northeast-1
cluster: your-cluster-name
service: your-service-name
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

Jsonnet形式の場合：

```jsonnet
{
  region: "ap-northeast-1",
  cluster: "your-cluster-name",
  service: "your-service-name",
  service_definition: "ecs-service-def.json",
  task_definition: "ecs-task-def.json",
  timeout: "10m"
}
```

## デプロイ

サービスをデプロイするには：

```console
$ ecspresso deploy
```

## 差分確認

デプロイ前に変更内容を確認するには：

```console
$ ecspresso diff
```

## タスク実行

一時的なタスクを実行するには：

```console
$ ecspresso run
```

## 設定検証

設定が正しいか検証するには：

```console
$ ecspresso verify
```

## デプロイフロー図

以下は基本的なデプロイフローを示しています：

```mermaid
graph TD
    A[設定ファイル準備] --> B[ecspresso diff で差分確認]
    B --> C{問題なし?}
    C -->|Yes| D[ecspresso deploy でデプロイ]
    C -->|No| E[設定ファイル修正]
    E --> B
    D --> F[デプロイ完了]
```
