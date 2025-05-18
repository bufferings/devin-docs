---
layout: default
title: 基本的な使い方
parent: クイックスタート
grand_parent: ecspresso
nav_order: 1
---

# 基本的な使い方

このページでは、ecspressoの基本的な使い方について説明します。

## 設定ファイルの作成

ecspressoを使用するには、まず設定ファイルを作成する必要があります。既存のECSサービスから設定ファイルを作成するには、`init`コマンドを使用します。

```bash
ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
```

このコマンドは以下のファイルを生成します：
- `ecspresso.yml` - ecspressoの設定ファイル
- `ecs-service-def.json` - ECSサービス定義ファイル
- `ecs-task-def.json` - ECSタスク定義ファイル

## 設定ファイルの構造

ecspressoの設定ファイル（YAML、JSON、またはJsonnet形式）は以下のような構造になっています：

```yaml
region: ap-northeast-1 # または環境変数 AWS_REGION
cluster: default
service: myservice
task_definition: ecs-task-def.json
service_definition: ecs-service-def.json
timeout: 5m # デフォルトは10m
```

主な設定項目：
- `region` - AWSリージョン
- `cluster` - ECSクラスター名
- `service` - ECSサービス名
- `task_definition` - タスク定義ファイルのパス
- `service_definition` - サービス定義ファイルのパス
- `timeout` - タイムアウト時間

## デプロイフロー

ecspressoのデプロイフローは以下のようになります：

```mermaid
graph TD
    A[設定ファイル読み込み] --> B[タスク定義ファイル読み込み]
    B --> C[テンプレート処理]
    C --> D[新しいタスク定義を登録]
    D --> E[サービス定義ファイル読み込み]
    E --> F[テンプレート処理]
    F --> G[サービスを更新]
    G --> H[サービスが安定するまで待機]
```

## 基本的なコマンド

### サービスのデプロイ

```bash
ecspresso deploy --config ecspresso.yml
```

このコマンドは以下の処理を行います：
1. タスク定義ファイルから新しいタスク定義を登録
2. サービス定義ファイルを使用してサービスを更新
3. サービスが安定するまで待機

### サービスのステータス確認

```bash
ecspresso status --config ecspresso.yml
```

このコマンドは、サービスの現在のステータスを表示します。

### 設定の差分確認

```bash
ecspresso diff --config ecspresso.yml
```

このコマンドは、現在のタスク定義・サービス定義と、実際に実行されているサービスとの差分を表示します。

### タスクの実行

```bash
ecspresso run --config ecspresso.yml
```

このコマンドは、タスク定義に基づいてタスクを実行します。

## テンプレート機能の使用

ecspressoのタスク定義ファイルとサービス定義ファイルはテンプレートとして処理されます。環境変数を使用してパラメータを動的に設定できます。

例えば、タスク定義ファイル内のイメージタグを環境変数から設定する場合：

```json
{
  "containerDefinitions": [
    {
      "name": "nginx",
      "image": "nginx:{{ must_env `IMAGE_TAG` }}",
      "essential": true
    }
  ]
}
```

デプロイ時に環境変数を指定します：

```bash
IMAGE_TAG=1.19 ecspresso deploy --config ecspresso.yml
```

## 環境変数ファイルの使用

複数の環境変数を設定する場合は、環境変数ファイルを使用できます：

```bash
ecspresso deploy --config ecspresso.yml --envfile=dev.env
```

環境変数ファイルの例（dev.env）：
```
IMAGE_TAG=1.19
DATABASE_URL=mysql://user:pass@localhost:3306/mydb
```

## 設定の検証

デプロイ前に設定を検証するには、`verify`コマンドを使用します：

```bash
ecspresso verify --config ecspresso.yml
```

このコマンドは以下の項目を検証します：
- ECSクラスターの存在
- IAMロールの存在と権限
- コンテナイメージの存在
- ターゲットグループの設定
- シークレットの存在と読み取り権限
- CloudWatchログの設定
