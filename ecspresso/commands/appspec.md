---
layout: default
title: appspec
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 13
---

# appspec

`appspec`コマンドは、AWS CodeDeployのAppSpecファイルを生成するためのコマンドです。Blue/Greenデプロイメントを使用する場合に必要なAppSpecファイルを簡単に作成できます。

## 基本的な使い方

```console
$ ecspresso appspec [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--task-definition` | タスク定義のARNまたはファミリー名:リビジョン | 最新のタスク定義 |
| `--output` | 出力ファイルのパス | 標準出力 |
| `--envfile` | 環境変数ファイルのパス | - |
| `--ext-str` | Jsonnet用の外部文字列値 | - |
| `--ext-code` | Jsonnet用の外部コード値 | - |

## 出力例

```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION>
        LoadBalancerInfo:
          ContainerName: web
          ContainerPort: 80
        PlatformVersion: 1.4.0
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso appspec --config ecspresso.yml
```

### 出力ファイルを指定

```console
$ ecspresso appspec --config ecspresso.yml --output appspec.yml
```

### 特定のタスク定義を指定

```console
$ ecspresso appspec --config ecspresso.yml --task-definition myservice:3
```

### 環境変数ファイルを使用

```console
$ ecspresso appspec --config ecspresso.yml --envfile production.env
```

### Jsonnet用の外部変数を指定

```console
$ ecspresso appspec --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3 --ext-str ENV=production
```

## AppSpecファイルの構造

AppSpecファイルは、AWS CodeDeployがBlue/Greenデプロイメントを実行するために使用するYAMLファイルです。主に以下の要素で構成されています：

1. **version**: AppSpecファイルのバージョン（常に`0.0`）
2. **Resources**: デプロイするリソースの定義
   - **TargetService**: ECSサービスの定義
     - **Type**: リソースタイプ（常に`AWS::ECS::Service`）
     - **Properties**: サービスのプロパティ
       - **TaskDefinition**: デプロイするタスク定義
       - **LoadBalancerInfo**: ロードバランサーの情報
         - **ContainerName**: コンテナ名
         - **ContainerPort**: コンテナポート
       - **PlatformVersion**: Fargateのプラットフォームバージョン
3. **Hooks**: デプロイメントライフサイクルイベントフック（オプション）

## デプロイメントライフサイクルイベントフック

AppSpecファイルには、デプロイメントライフサイクルの各段階で実行するLambda関数を指定できます。

```yaml
Hooks:
  - BeforeInstall: "arn:aws:lambda:ap-northeast-1:123456789012:function:BeforeInstallHook"
  - AfterInstall: "arn:aws:lambda:ap-northeast-1:123456789012:function:AfterInstallHook"
  - AfterAllowTestTraffic: "arn:aws:lambda:ap-northeast-1:123456789012:function:AfterAllowTestTrafficHook"
  - BeforeAllowTraffic: "arn:aws:lambda:ap-northeast-1:123456789012:function:BeforeAllowTrafficHook"
  - AfterAllowTraffic: "arn:aws:lambda:ap-northeast-1:123456789012:function:AfterAllowTrafficHook"
```

各フックの役割：

- **BeforeInstall**: 新しいタスクセットを作成する前に実行
- **AfterInstall**: 新しいタスクセットを作成した後に実行
- **AfterAllowTestTraffic**: テストリスナーにトラフィックを転送した後に実行
- **BeforeAllowTraffic**: 本番リスナーにトラフィックを転送する前に実行
- **AfterAllowTraffic**: 本番リスナーにトラフィックを転送した後に実行

## Blue/Greenデプロイメントの設定

Blue/Greenデプロイメントを使用するには、以下の設定が必要です：

1. サービス定義ファイルで`deploymentController.type`を`CODE_DEPLOY`に設定
2. ロードバランサーを設定
3. `ecspresso.yml`ファイルで`codedeploy`セクションを設定
4. `appspec`コマンドでAppSpecファイルを生成

ecspresso.ymlの設定例：

```yaml
region: ap-northeast-1
cluster: my-cluster
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
codedeploy:
  application_name: myapp
  deployment_group_name: myapp-group
  deployment_config_name: CodeDeployDefault.ECSAllAtOnce
  auto_rollback: true
```

サービス定義ファイルの例：

```json
{
  "serviceName": "myservice",
  "desiredCount": 3,
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
  "deploymentController": {
    "type": "CODE_DEPLOY"
  },
  "loadBalancers": [
    {
      "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/blue/abcdef1234567890",
      "containerName": "web",
      "containerPort": 80
    }
  ]
}
```

## CI/CDパイプラインでの使用

`appspec`コマンドは、CI/CDパイプラインでBlue/Greenデプロイメントを自動化するのに役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          # AppSpecファイルを生成
          ecspresso appspec --config ecspresso.yml --output appspec.yml
          
          # タスク定義を登録
          ecspresso register --config ecspresso.yml --output task-definition-arn.txt
          
          # CodeDeployでデプロイ
          aws deploy create-deployment \
            --application-name myapp \
            --deployment-group-name myapp-group \
            --revision revisionType=AppSpecContent,appSpecContent="{content: $(cat appspec.yml | jq -R -s .)}" \
            --description "Deployment via GitHub Actions"
```

## 注意事項

- `appspec`コマンドは、Blue/Greenデプロイメントを使用する場合にのみ必要です
- AppSpecファイルには、`<TASK_DEFINITION>`プレースホルダーが含まれています。これは、AWS CodeDeployによって自動的に置き換えられます
- デプロイメントライフサイクルイベントフックを使用する場合は、Lambda関数を作成し、ARNをAppSpecファイルに指定する必要があります
- Blue/Greenデプロイメントを使用するには、ロードバランサーが必要です
- `ecspresso deploy`コマンドを使用する場合は、AppSpecファイルを手動で作成する必要はありません。ecspressoが自動的に処理します

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [register](./register.html) - タスク定義を登録
- [revisions](./revisions.html) - タスク定義のリビジョンを一覧表示
