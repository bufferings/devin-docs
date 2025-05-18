---
layout: default
title: よくあるユースケース
parent: クイックスタート
grand_parent: ecspresso
nav_order: 2
---

# よくあるユースケース

このページでは、ecspressoの一般的なユースケースと実践的な使用例を紹介します。

## Blue/Greenデプロイ

Blue/Greenデプロイは、ダウンタイムなしでアプリケーションを更新するための手法です。ecspressoは、AWS CodeDeployと連携してBlue/Greenデプロイをサポートしています。

### 設定方法

1. サービス定義ファイルで、デプロイメントコントローラーをCodeDeployに設定します：

```jsonc
{
  "deploymentController": {
    "type": "CODE_DEPLOY"
  },
  "loadBalancers": [
    {
      "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/blue/0123456789abcdef",
      "containerName": "web",
      "containerPort": 80
    }
  ]
}
```

2. ecspresso.ymlにCodeDeployの設定を追加します：

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
codedeploy:
  application_name: AppECS-default-myservice
  deployment_group_name: DgpECS-default-myservice
  deployment_config_name: CodeDeployDefault.ECSAllAtOnce
  auto_rollback_on_error: true
```

3. デプロイを実行します：

```console
$ ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
```

## 自動スケーリングの設定

ecspressoを使用して、ECSサービスの自動スケーリングを設定できます。

### 設定方法

1. サービス定義ファイルに自動スケーリングの設定を追加します：

```jsonc
{
  "deploymentConfiguration": {
    "maximumPercent": 200,
    "minimumHealthyPercent": 100
  }
}
```

2. `scale`コマンドを使用して、自動スケーリングを設定します：

```console
$ ecspresso scale --tasks 2 --min-tasks 1 --max-tasks 10
```

3. CloudWatchアラームに基づいたスケーリングポリシーを設定するには、AWS Management Consoleまたはaws-cliを使用します。

## 環境変数を使用した複数環境の管理

異なる環境（開発、ステージング、本番）で同じアプリケーションを管理するために、環境変数ファイルを使用できます。

### 設定方法

1. 環境ごとに環境変数ファイルを作成します：

```console
$ cat .env.dev
IMAGE_TAG=dev
MIN_CAPACITY=1
MAX_CAPACITY=2

$ cat .env.prod
IMAGE_TAG=v1.2.3
MIN_CAPACITY=2
MAX_CAPACITY=10
```

2. タスク定義ファイルでテンプレート変数を使用します：

```jsonc
{
  "containerDefinitions": [
    {
      "name": "web",
      "image": "myapp:{{ must_env `IMAGE_TAG` }}",
      // ...
    }
  ]
}
```

3. 環境変数ファイルを指定してデプロイします：

```console
$ ecspresso deploy --config ecspresso.yml --envfile .env.dev
$ ecspresso deploy --config ecspresso.yml --envfile .env.prod
```

## 一時的なタスクの実行

データベースマイグレーションやバッチ処理などの一時的なタスクを実行するには、`run`コマンドを使用します。

### 使用例

1. 基本的なタスク実行：

```console
$ ecspresso run --task-def ecs-task-def.json
```

2. コマンドのオーバーライド：

```console
$ ecspresso run --task-def ecs-task-def.json --overrides '{"containerOverrides":[{"name":"web","command":["./migrate.sh"]}]}'
```

3. 実行結果の確認：

```console
$ ecspresso run --task-def ecs-task-def.json --watch
```

## デプロイ戦略のカスタマイズ

ecspressoでは、様々なデプロイ戦略をカスタマイズできます。

### 強制的な新規デプロイ

現在のタスク定義を使用して、強制的に新しいデプロイを開始します：

```console
$ ecspresso deploy --force-new-deployment
```

### タスク定義の更新をスキップ

サービスの設定のみを更新し、タスク定義の更新をスキップします：

```console
$ ecspresso deploy --skip-task-definition
```

### 自動スケーリングの再開

デプロイ後に自動スケーリングを再開します：

```console
$ ecspresso deploy --resume-auto-scaling
```

## デプロイのロールバック

デプロイに問題が発生した場合、以前のバージョンにロールバックできます。

### 手動ロールバック

1. 以前のリビジョンを確認します：

```console
$ ecspresso revisions
```

2. 特定のリビジョンにロールバックします：

```console
$ ecspresso rollback --revision 3
```

### 自動ロールバック

デプロイ中にエラーが発生した場合、自動的にロールバックするように設定できます：

```console
$ ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
```

## Jsonnetを使用した高度なテンプレート

Jsonnetを使用すると、より柔軟で再利用可能な設定ファイルを作成できます。

### 使用例

1. Jsonnetファイルの作成：

```jsonnet
// ecs-task-def.jsonnet
local env = std.extVar('env');
local common = import 'common.libsonnet';

{
  family: "myservice-" + env,
  containerDefinitions: [
    {
      name: "web",
      image: "myapp:" + std.extVar('IMAGE_TAG'),
      essential: true,
      portMappings: [
        {
          containerPort: 80,
          hostPort: 80,
          protocol: "tcp"
        }
      ],
      logConfiguration: common.logConfiguration(env),
      environment: common.environment(env)
    }
  ],
  executionRoleArn: common.roles.executionRole,
  networkMode: "awsvpc",
  requiresCompatibilities: [
    "FARGATE"
  ],
  cpu: if env == "prod" then "512" else "256",
  memory: if env == "prod" then "1024" else "512"
}
```

2. 外部変数を指定してデプロイ：

```console
$ ecspresso deploy --config ecspresso.yml --ext-str env=prod --ext-str IMAGE_TAG=v1.2.3
```

## VPC Latticeとの統合

ecspressoは、VPC Latticeとの統合をサポートしています。

### 設定方法

1. サービス定義ファイルにVPC Latticeの設定を追加します：

```jsonc
{
  "vpcLatticeConfigurations": [
    {
      "targetGroupArn": "arn:aws:vpc-lattice:ap-northeast-1:123456789012:targetgroup/tg-0123456789abcdef",
      "containerName": "web",
      "containerPort": 80
    }
  ]
}
```

2. 通常通りデプロイします：

```console
$ ecspresso deploy --config ecspresso.yml
```

## EBSボリュームの使用

ecspressoは、EBSボリュームの設定もサポートしています。

### 設定方法

1. サービス定義ファイルにボリューム設定を追加します：

```jsonc
{
  "volumeConfigurations": [
    {
      "name": "data",
      "managedEBSVolume": {
        "encrypted": true,
        "kmsKeyId": "arn:aws:kms:ap-northeast-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab",
        "sizeInGiB": 20,
        "snapshotId": "snap-0123456789abcdef0",
        "volumeType": "gp3",
        "iops": 3000,
        "throughput": 125,
        "tagSpecifications": [
          {
            "resourceType": "volume",
            "tags": [
              {
                "key": "Name",
                "value": "myservice-data"
              }
            ]
          }
        ]
      }
    }
  ]
}
```

2. タスク定義ファイルでボリュームを参照します：

```jsonc
{
  "volumes": [
    {
      "name": "data"
    }
  ],
  "containerDefinitions": [
    {
      "name": "web",
      "mountPoints": [
        {
          "sourceVolume": "data",
          "containerPath": "/data",
          "readOnly": false
        }
      ]
    }
  ]
}
```

3. 通常通りデプロイします：

```console
$ ecspresso deploy --config ecspresso.yml
```

## 次のステップ

- [コマンドリファレンス](../commands/index.html)を参照して、各コマンドの詳細を確認しましょう
- [実践ガイド](../practical/index.html)を参照して、より高度な使用方法を学びましょう
