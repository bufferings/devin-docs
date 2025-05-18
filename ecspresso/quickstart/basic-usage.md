---
layout: default
title: 基本的な使い方
parent: クイックスタート
grand_parent: ecspresso
nav_order: 1
---

# 基本的な使い方

このガイドでは、ecspressoの基本的な使い方を説明します。

## 既存のECSサービスからの設定ファイル作成

ecspressoを使い始める最も簡単な方法は、既存のECSサービスから設定ファイルを作成することです。`init`コマンドを使用して、必要な設定ファイルを生成できます。

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
2023/01/01 12:00:00 myservice/default save service definition to ecs-service-def.json
2023/01/01 12:00:00 myservice/default save task definition to ecs-task-def.json
2023/01/01 12:00:00 myservice/default save config to ecspresso.yml
```

このコマンドは、以下の3つのファイルを生成します：

1. **ecspresso.yml**: ecspressoの設定ファイル
2. **ecs-service-def.json**: ECSサービス定義ファイル
3. **ecs-task-def.json**: ECSタスク定義ファイル

## 設定ファイルの構造

`ecspresso.yml`は、ecspressoの主要な設定ファイルです。基本的な構造は以下の通りです：

```yaml
region: ap-northeast-1 # または環境変数 AWS_REGION
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 5m # デフォルトは10m
```

主な設定項目：

- **region**: AWSリージョン
- **cluster**: ECSクラスター名
- **service**: ECSサービス名
- **service_definition**: サービス定義ファイルのパス
- **task_definition**: タスク定義ファイルのパス
- **timeout**: コマンド実行のタイムアウト時間

## サービスのデプロイ

設定ファイルを作成したら、`deploy`コマンドを使用してサービスをデプロイできます。

```console
$ ecspresso deploy --config ecspresso.yml
2023/01/01 12:00:00 myservice/default Starting deploy
Service: myservice
Cluster: default
TaskDefinition: myservice:3
Deployments:
    PRIMARY myservice:3 desired:1 pending:0 running:1
Events:
2023/01/01 12:00:00 myservice/default Creating a new task definition by ecs-task-def.json
2023/01/01 12:00:00 myservice/default Registering a new task definition...
2023/01/01 12:00:00 myservice/default Task definition is registered myservice:4
2023/01/01 12:00:00 myservice/default Updating service...
2023/01/01 12:00:00 myservice/default Waiting for service stable...(it will take a few minutes)
2023/01/01 12:00:00 myservice/default  PRIMARY myservice:4 desired:1 pending:0 running:1
2023/01/01 12:00:00 myservice/default Service is stable now. Completed!
```

`deploy`コマンドは、以下の処理を行います：

1. タスク定義ファイルから新しいタスク定義を登録
2. サービス定義ファイルに基づいてサービスを更新
3. サービスが安定状態になるまで待機

## サービスの状態確認

`status`コマンドを使用して、サービスの現在の状態を確認できます。

```console
$ ecspresso status --config ecspresso.yml
Service: myservice
Cluster: default
TaskDefinition: myservice:4
Deployments:
    PRIMARY myservice:4 desired:1 pending:0 running:1
Events:
2023/01/01 12:00:00 (service myservice) has reached a steady state.
```

## テンプレート機能の使用

ecspressoは、タスク定義ファイルとサービス定義ファイルでテンプレート機能をサポートしています。例えば、イメージタグを環境変数から取得するには、以下のようにします：

```diff
-  "image": "nginx:latest",
+  "image": "nginx:{{ must_env `IMAGE_TAG` }}",
```

そして、環境変数を設定してデプロイします：

```console
$ IMAGE_TAG=stable ecspresso deploy --config ecspresso.yml
```

### テンプレート関数

ecspressoは、以下のテンプレート関数を提供しています：

1. **env**: 環境変数の値を取得（デフォルト値を指定可能）
   ```
   "{{ env `NAME` `default value` }}"
   ```

2. **must_env**: 環境変数の値を取得（未設定の場合はエラー）
   ```
   "{{ must_env `NAME` }}"
   ```

3. **json_escape**: JSON文字列としてエスケープ
   ```
   "{{ must_env `JSON_VALUE` | json_escape }}"
   ```

## 環境変数ファイルの使用

複数の環境変数を設定する場合は、環境変数ファイルを使用できます。

```console
$ cat .env.prod
IMAGE_TAG=v1.2.3
MIN_CAPACITY=2
MAX_CAPACITY=10

$ ecspresso deploy --config ecspresso.yml --envfile .env.prod
```

## Jsonnetの使用

ecspressoは、JSONの代わりにJsonnetファイル形式もサポートしています。Jsonnetを使用すると、より柔軟な設定が可能になります。

```jsonnet
// ecs-task-def.jsonnet
{
  family: "myservice",
  containerDefinitions: [
    {
      name: "web",
      image: "nginx:" + std.extVar("IMAGE_TAG"),
      essential: true,
      portMappings: [
        {
          containerPort: 80,
          hostPort: 80,
          protocol: "tcp"
        }
      ],
      logConfiguration: {
        logDriver: "awslogs",
        options: {
          "awslogs-group": "/ecs/myservice",
          "awslogs-region": "ap-northeast-1",
          "awslogs-stream-prefix": "web"
        }
      }
    }
  ],
  executionRoleArn: "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  networkMode: "awsvpc",
  requiresCompatibilities: [
    "FARGATE"
  ],
  cpu: "256",
  memory: "512"
}
```

Jsonnetファイルを使用する場合は、拡張子を`.jsonnet`にして、設定ファイルで指定します：

```yaml
# ecspresso.yml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.jsonnet
task_definition: ecs-task-def.jsonnet
```

外部変数を渡してデプロイします：

```console
$ ecspresso deploy --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3
```

## ドライランモード

実際に変更を適用する前に、変更内容を確認するには、`--dry-run`オプションを使用します。

```console
$ ecspresso deploy --config ecspresso.yml --dry-run
```

## 差分の確認

現在のサービスとタスク定義と、ローカルの設定ファイルとの差分を確認するには、`diff`コマンドを使用します。

```console
$ ecspresso diff --config ecspresso.yml
```

## 次のステップ

- [よくあるユースケース](./use-cases.html)を参照して、より実践的な使用例を学びましょう
- [コマンドリファレンス](../commands/index.html)を参照して、各コマンドの詳細を確認しましょう
