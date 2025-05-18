---
layout: default
title: 複数環境での運用
parent: 実践ガイド
grand_parent: ecspresso
nav_order: 2
---

# 複数環境での運用

ecspressoは、開発環境、ステージング環境、本番環境など、複数の環境でECSサービスを管理するための機能を提供しています。このガイドでは、複数環境でecspressoを効果的に使用する方法を説明します。

## 目次

1. [環境ごとの設定ファイル](#環境ごとの設定ファイル)
2. [環境変数ファイルの使用](#環境変数ファイルの使用)
3. [テンプレート変数](#テンプレート変数)
4. [Jsonnetによる設定管理](#jsonnetによる設定管理)
5. [環境ごとのデプロイ戦略](#環境ごとのデプロイ戦略)
6. [CI/CDパイプラインでの環境分離](#cicdパイプラインでの環境分離)
7. [ベストプラクティス](#ベストプラクティス)

## 環境ごとの設定ファイル

複数の環境を管理する最も簡単な方法は、環境ごとに別々の設定ファイルを用意することです。

```
.
├── ecspresso.dev.yml     # 開発環境用の設定ファイル
├── ecspresso.staging.yml # ステージング環境用の設定ファイル
├── ecspresso.prod.yml    # 本番環境用の設定ファイル
├── ecs-task-def.json     # 共通のタスク定義ファイル
└── ecs-service-def.json  # 共通のサービス定義ファイル
```

各環境用の設定ファイルの例：

```yaml
# ecspresso.dev.yml
region: ap-northeast-1
cluster: dev-cluster
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

```yaml
# ecspresso.prod.yml
region: ap-northeast-1
cluster: prod-cluster
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 20m
```

環境ごとの設定ファイルを使用してデプロイする例：

```console
# 開発環境にデプロイ
$ ecspresso deploy --config ecspresso.dev.yml

# ステージング環境にデプロイ
$ ecspresso deploy --config ecspresso.staging.yml

# 本番環境にデプロイ
$ ecspresso deploy --config ecspresso.prod.yml
```

## 環境変数ファイルの使用

`--envfile`オプションを使用して、環境ごとに異なる環境変数ファイルを指定できます。

```
.
├── ecspresso.yml         # 共通の設定ファイル
├── ecs-task-def.json     # 共通のタスク定義ファイル
├── ecs-service-def.json  # 共通のサービス定義ファイル
├── dev.env               # 開発環境用の環境変数ファイル
├── staging.env           # ステージング環境用の環境変数ファイル
└── prod.env              # 本番環境用の環境変数ファイル
```

環境変数ファイルの例：

```
# dev.env
CLUSTER=dev-cluster
DESIRED_COUNT=1
MIN_CAPACITY=1
MAX_CAPACITY=2
MEMORY=512
CPU=256
LOG_LEVEL=debug
```

```
# prod.env
CLUSTER=prod-cluster
DESIRED_COUNT=3
MIN_CAPACITY=2
MAX_CAPACITY=10
MEMORY=1024
CPU=512
LOG_LEVEL=info
```

環境変数ファイルを使用してデプロイする例：

```console
# 開発環境にデプロイ
$ ecspresso deploy --config ecspresso.yml --envfile dev.env

# ステージング環境にデプロイ
$ ecspresso deploy --config ecspresso.yml --envfile staging.env

# 本番環境にデプロイ
$ ecspresso deploy --config ecspresso.yml --envfile prod.env
```

## テンプレート変数

ecspressoは、タスク定義ファイルとサービス定義ファイル内でテンプレート変数を使用することをサポートしています。これにより、環境ごとに異なる値を設定できます。

タスク定義ファイルの例：

```json
{
  "family": "myservice",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:{{ must_env `IMAGE_TAG` }}",
      "essential": true,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/myservice-{{ env `ENV` `dev` }}",
          "awslogs-region": "{{ must_env `AWS_REGION` }}",
          "awslogs-stream-prefix": "web"
        }
      },
      "environment": [
        {
          "name": "ENV",
          "value": "{{ env `ENV` `dev` }}"
        },
        {
          "name": "LOG_LEVEL",
          "value": "{{ env `LOG_LEVEL` `info` }}"
        }
      ],
      "cpu": {{ env `CPU` `256` }},
      "memory": {{ env `MEMORY` `512` }},
      "memoryReservation": {{ env `MEMORY_RESERVATION` `256` }}
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "cpu": "{{ env `CPU` `256` }}",
  "memory": "{{ env `MEMORY` `512` }}"
}
```

サービス定義ファイルの例：

```json
{
  "serviceName": "myservice",
  "desiredCount": {{ env `DESIRED_COUNT` `1` }},
  "launchType": "FARGATE",
  "networkConfiguration": {
    "awsvpcConfiguration": {
      "subnets": [
        "{{ must_env `SUBNET_1` }}",
        "{{ must_env `SUBNET_2` }}"
      ],
      "securityGroups": [
        "{{ must_env `SECURITY_GROUP` }}"
      ],
      "assignPublicIp": "{{ env `ASSIGN_PUBLIC_IP` `DISABLED` }}"
    }
  },
  "deploymentConfiguration": {
    "deploymentCircuitBreaker": {
      "enable": {{ env `CIRCUIT_BREAKER_ENABLE` `false` }},
      "rollback": {{ env `CIRCUIT_BREAKER_ROLLBACK` `false` }}
    }
  },
  "loadBalancers": [
    {
      "targetGroupArn": "{{ must_env `TARGET_GROUP_ARN` }}",
      "containerName": "web",
      "containerPort": 80
    }
  ]
}
```

テンプレート変数を使用してデプロイする例：

```console
# 環境変数を直接指定
$ export IMAGE_TAG=v1.2.3
$ export ENV=prod
$ export AWS_REGION=ap-northeast-1
$ export DESIRED_COUNT=3
$ ecspresso deploy --config ecspresso.yml

# --ext-strオプションを使用
$ ecspresso deploy --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3 --ext-str ENV=prod
```

## Jsonnetによる設定管理

より複雑な設定管理には、Jsonnetを使用できます。Jsonnetは、JSONのスーパーセットであり、変数、条件分岐、関数などをサポートしています。

```
.
├── ecspresso.yml         # 共通の設定ファイル
├── ecs-task-def.jsonnet  # Jsonnet形式のタスク定義ファイル
├── ecs-service-def.jsonnet # Jsonnet形式のサービス定義ファイル
└── lib/
    ├── common.libsonnet  # 共通のライブラリ
    ├── dev.libsonnet     # 開発環境用の設定
    ├── staging.libsonnet # ステージング環境用の設定
    └── prod.libsonnet    # 本番環境用の設定
```

タスク定義ファイルの例（Jsonnet）：

```jsonnet
// ecs-task-def.jsonnet
local env = std.extVar('ENV');
local common = import 'lib/common.libsonnet';
local envConfig = import 'lib/' + env + '.libsonnet';

{
  family: "myservice",
  containerDefinitions: [
    {
      name: "web",
      image: "nginx:" + std.extVar('IMAGE_TAG'),
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
          "awslogs-group": "/ecs/myservice-" + env,
          "awslogs-region": envConfig.region,
          "awslogs-stream-prefix": "web"
        }
      },
      environment: common.environment(env) + [
        {
          name: "LOG_LEVEL",
          value: envConfig.logLevel
        }
      ],
      cpu: envConfig.container.cpu,
      memory: envConfig.container.memory,
      memoryReservation: envConfig.container.memoryReservation
    }
  ],
  executionRoleArn: common.roles.executionRole,
  networkMode: "awsvpc",
  requiresCompatibilities: [
    "FARGATE"
  ],
  cpu: envConfig.task.cpu,
  memory: envConfig.task.memory
}
```

共通ライブラリの例：

```jsonnet
// lib/common.libsonnet
{
  environment(env): [
    {
      name: "ENV",
      value: env
    }
  ],
  roles: {
    executionRole: "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
    taskRole: "arn:aws:iam::123456789012:role/ecsTaskRole"
  }
}
```

環境設定の例：

```jsonnet
// lib/dev.libsonnet
{
  region: "ap-northeast-1",
  logLevel: "debug",
  container: {
    cpu: 256,
    memory: 512,
    memoryReservation: 256
  },
  task: {
    cpu: "256",
    memory: "512"
  },
  desiredCount: 1,
  subnets: [
    "subnet-12345678",
    "subnet-87654321"
  ],
  securityGroups: [
    "sg-12345678"
  ],
  assignPublicIp: "ENABLED",
  circuitBreaker: {
    enable: false,
    rollback: false
  }
}
```

```jsonnet
// lib/prod.libsonnet
{
  region: "ap-northeast-1",
  logLevel: "info",
  container: {
    cpu: 512,
    memory: 1024,
    memoryReservation: 512
  },
  task: {
    cpu: "512",
    memory: "1024"
  },
  desiredCount: 3,
  subnets: [
    "subnet-abcdefgh",
    "subnet-hgfedcba"
  ],
  securityGroups: [
    "sg-abcdefgh"
  ],
  assignPublicIp: "DISABLED",
  circuitBreaker: {
    enable: true,
    rollback: true
  }
}
```

Jsonnetを使用してデプロイする例：

```console
# 開発環境にデプロイ
$ ecspresso deploy --config ecspresso.yml --ext-str ENV=dev --ext-str IMAGE_TAG=v1.2.3

# 本番環境にデプロイ
$ ecspresso deploy --config ecspresso.yml --ext-str ENV=prod --ext-str IMAGE_TAG=v1.2.3
```

## 環境ごとのデプロイ戦略

環境ごとに異なるデプロイ戦略を使用することができます。

### 開発環境

開発環境では、迅速なデプロイと頻繁な更新が重要です。

```console
# 強制的に新しいデプロイメントを作成
$ ecspresso deploy --config ecspresso.dev.yml --force-new-deployment
```

### ステージング環境

ステージング環境では、本番環境と同様のデプロイプロセスをテストします。

```console
# Blue/Greenデプロイメントを使用
$ ecspresso deploy --config ecspresso.staging.yml
```

### 本番環境

本番環境では、安全なデプロイと自動ロールバックが重要です。

```console
# ロールバックイベントを指定
$ ecspresso deploy --config ecspresso.prod.yml --rollback-events DEPLOYMENT_FAILURE
```

## CI/CDパイプラインでの環境分離

CI/CDパイプラインで複数環境を管理する例を示します。

### GitHub Actionsの例

```yaml
name: Deploy to ECS

on:
  push:
    branches:
      - develop  # 開発環境へのデプロイ
      - staging  # ステージング環境へのデプロイ
      - main     # 本番環境へのデプロイ

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: {% raw %}${{ secrets.AWS_ACCESS_KEY_ID }}{% endraw %}
          aws-secret-access-key: {% raw %}${{ secrets.AWS_SECRET_ACCESS_KEY }}{% endraw %}
          aws-region: ap-northeast-1
      
      - name: Setup ecspresso
        uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      
      - name: Set environment variables
        id: env
        run: |
          if [[ $GITHUB_REF == 'refs/heads/develop' ]]; then
            echo "::set-output name=env::dev"
            echo "::set-output name=config::ecspresso.dev.yml"
          elif [[ $GITHUB_REF == 'refs/heads/staging' ]]; then
            echo "::set-output name=env::staging"
            echo "::set-output name=config::ecspresso.staging.yml"
          elif [[ $GITHUB_REF == 'refs/heads/main' ]]; then
            echo "::set-output name=env::prod"
            echo "::set-output name=config::ecspresso.prod.yml"
          fi
      
      - name: Deploy to ECS
        env:
          IMAGE_TAG: {% raw %}${{ github.sha }}{% endraw %}
          ENV: {% raw %}${{ steps.env.outputs.env }}{% endraw %}
          CONFIG: {% raw %}${{ steps.env.outputs.config }}{% endraw %}
        run: |
          ecspresso deploy --config $CONFIG --ext-str IMAGE_TAG=$IMAGE_TAG --ext-str ENV=$ENV
          ecspresso wait --config $CONFIG --timeout 10m
```

## ベストプラクティス

### 1. 環境ごとの設定を分離

環境ごとに異なる設定を明確に分離します。

```
.
├── config/
│   ├── dev/
│   │   ├── ecspresso.yml
│   │   ├── ecs-task-def.json
│   │   └── ecs-service-def.json
│   ├── staging/
│   │   ├── ecspresso.yml
│   │   ├── ecs-task-def.json
│   │   └── ecs-service-def.json
│   └── prod/
│       ├── ecspresso.yml
│       ├── ecs-task-def.json
│       └── ecs-service-def.json
└── scripts/
    ├── deploy-dev.sh
    ├── deploy-staging.sh
    └── deploy-prod.sh
```

### 2. 環境変数の管理

機密情報は環境変数として管理し、リポジトリにコミットしないようにします。

```console
# .envファイルを使用
$ ecspresso deploy --config ecspresso.yml --envfile .env
```

### 3. デプロイスクリプトの作成

環境ごとのデプロイスクリプトを作成して、デプロイプロセスを簡素化します。

```bash
#!/bin/bash
# scripts/deploy-prod.sh
set -e

# 環境変数の読み込み
source .env.prod

# デプロイ前の差分確認
ecspresso diff --config config/prod/ecspresso.yml

# 確認プロンプト
read -p "本番環境にデプロイしますか？ (y/n): " confirm
if [[ $confirm != "y" ]]; then
  echo "デプロイをキャンセルしました"
  exit 1
fi

# デプロイ実行
ecspresso deploy --config config/prod/ecspresso.yml --rollback-events DEPLOYMENT_FAILURE
ecspresso wait --config config/prod/ecspresso.yml --timeout 20m
```

### 4. 環境ごとのタスク定義の検証

デプロイ前に、環境ごとのタスク定義を検証します。

```console
# 開発環境の検証
$ ecspresso verify --config ecspresso.dev.yml

# 本番環境の検証
$ ecspresso verify --config ecspresso.prod.yml
```

### 5. 環境ごとのリソース制限

環境ごとに適切なリソース制限を設定します。

```json
// 開発環境のタスク定義
{
  "cpu": "256",
  "memory": "512"
}

// 本番環境のタスク定義
{
  "cpu": "1024",
  "memory": "2048"
}
```

### 6. 環境ごとのロギング設定

環境ごとに適切なロギング設定を行います。

```json
// 開発環境のロギング設定
{
  "logConfiguration": {
    "logDriver": "awslogs",
    "options": {
      "awslogs-group": "/ecs/myservice-dev",
      "awslogs-region": "ap-northeast-1",
      "awslogs-stream-prefix": "web"
    }
  }
}

// 本番環境のロギング設定
{
  "logConfiguration": {
    "logDriver": "awslogs",
    "options": {
      "awslogs-group": "/ecs/myservice-prod",
      "awslogs-region": "ap-northeast-1",
      "awslogs-stream-prefix": "web"
    }
  }
}
```

### 7. 環境ごとのスケーリング設定

環境ごとに適切なスケーリング設定を行います。

```json
// 開発環境のスケーリング設定
{
  "desiredCount": 1,
  "deploymentConfiguration": {
    "maximumPercent": 200,
    "minimumHealthyPercent": 100
  }
}

// 本番環境のスケーリング設定
{
  "desiredCount": 3,
  "deploymentConfiguration": {
    "maximumPercent": 150,
    "minimumHealthyPercent": 100
  }
}
```

## 関連コマンド

- [deploy](../commands/deploy.html) - サービスをデプロイ
- [diff](../commands/diff.html) - ローカルの設定ファイルと現在のサービスとの差分を表示
- [verify](../commands/verify.html) - 設定ファイルの検証
- [render](../commands/render.html) - 設定ファイルをレンダリングして標準出力に表示
