---
layout: default
title: render
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 2
---

# render

`render`コマンドは、タスク定義ファイルとサービス定義ファイルをレンダリングして標準出力に表示するためのコマンドです。テンプレート変数の展開結果を確認したり、Jsonnetファイルの評価結果を確認したりするのに役立ちます。

## 基本的な使い方

```console
$ ecspresso render [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--envfile` | 環境変数ファイルのパス | - |
| `--ext-str` | Jsonnet用の外部文字列値 | - |
| `--ext-code` | Jsonnet用の外部コード値 | - |
| `--task-definition` | タスク定義のみをレンダリング | `false` |
| `--service-definition` | サービス定義のみをレンダリング | `false` |
| `--appspec` | AppSpecのみをレンダリング | `false` |

## 出力例

### タスク定義のレンダリング

```console
$ ecspresso render --config ecspresso.yml --task-definition
```

出力：

```json
{
  "family": "myservice",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:v1.2.3",
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
          "awslogs-group": "/ecs/myservice",
          "awslogs-region": "ap-northeast-1",
          "awslogs-stream-prefix": "web"
        }
      }
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "cpu": "256",
  "memory": "512"
}
```

### サービス定義のレンダリング

```console
$ ecspresso render --config ecspresso.yml --service-definition
```

出力：

```json
{
  "serviceName": "myservice",
  "desiredCount": 2,
  "launchType": "FARGATE",
  "networkConfiguration": {
    "awsvpcConfiguration": {
      "subnets": [
        "subnet-12345678",
        "subnet-87654321"
      ],
      "securityGroups": [
        "sg-12345678"
      ],
      "assignPublicIp": "DISABLED"
    }
  },
  "loadBalancers": [
    {
      "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890",
      "containerName": "web",
      "containerPort": 80
    }
  ]
}
```

### AppSpecのレンダリング

```console
$ ecspresso render --config ecspresso.yml --appspec
```

出力：

```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:4"
        LoadBalancerInfo:
          ContainerName: "web"
          ContainerPort: 80
Hooks:
  - BeforeInstall: "LambdaFunctionToValidateBeforeInstall"
  - AfterInstall: "LambdaFunctionToValidateAfterInstall"
```

## 使用例

### 環境変数ファイルを使用

```console
$ ecspresso render --config ecspresso.yml --envfile production.env
```

### Jsonnet用の外部変数を指定

```console
$ ecspresso render --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3 --ext-str ENV=production
```

### タスク定義のみをレンダリング

```console
$ ecspresso render --config ecspresso.yml --task-definition
```

### サービス定義のみをレンダリング

```console
$ ecspresso render --config ecspresso.yml --service-definition
```

### AppSpecのみをレンダリング

```console
$ ecspresso render --config ecspresso.yml --appspec
```

## テンプレート変数の展開

`render`コマンドは、タスク定義ファイルとサービス定義ファイル内のテンプレート変数を展開します。例えば、以下のようなタスク定義ファイルがある場合：

```json
{
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:{{ must_env `IMAGE_TAG` }}",
      "environment": [
        {
          "name": "ENV",
          "value": "{{ env `ENV` `development` }}"
        }
      ]
    }
  ]
}
```

環境変数`IMAGE_TAG=v1.2.3`を設定して`render`コマンドを実行すると、以下のように展開されます：

```json
{
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:v1.2.3",
      "environment": [
        {
          "name": "ENV",
          "value": "development"
        }
      ]
    }
  ]
}
```

## Jsonnetの評価

`render`コマンドは、Jsonnetファイルを評価して結果を表示します。例えば、以下のようなJsonnetファイルがある場合：

```jsonnet
// ecs-task-def.jsonnet
local env = std.extVar('ENV');
local common = import 'common.libsonnet';

{
  family: "myservice-" + env,
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

外部変数を指定して`render`コマンドを実行すると、以下のように評価されます：

```console
$ ecspresso render --config ecspresso.yml --task-definition --ext-str ENV=prod --ext-str IMAGE_TAG=v1.2.3
```

出力：

```json
{
  "family": "myservice-prod",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:v1.2.3",
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
          "awslogs-group": "/ecs/myservice-prod",
          "awslogs-region": "ap-northeast-1",
          "awslogs-stream-prefix": "web"
        }
      },
      "environment": [
        {
          "name": "ENV",
          "value": "prod"
        }
      ]
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "cpu": "512",
  "memory": "1024"
}
```

## 注意事項

- `render`コマンドは、ローカルファイルのレンダリングのみを行い、AWSリソースとの通信は行いません
- テンプレート変数を使用している場合は、`--envfile`または`--ext-str`オプションで値を指定する必要があります
- Jsonnetファイルを使用している場合は、`--ext-str`または`--ext-code`オプションで外部変数を渡すことができます
- `--task-definition`、`--service-definition`、`--appspec`オプションを指定しない場合は、すべてのファイルがレンダリングされます
- `render`コマンドは、デプロイ前にテンプレート変数の展開結果を確認するために使用することをお勧めします

## 関連コマンド

- [init](./init.html) - 既存のECSサービスから設定ファイルを生成
- [verify](./verify.html) - 設定ファイルの検証
- [deploy](./deploy.html) - サービスをデプロイ
- [diff](./diff.html) - ローカルの設定ファイルと現在のサービスとの差分を表示
