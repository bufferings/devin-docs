---
layout: default
title: appspec
nav_order: 1
parent: コマンドリファレンス
grand_parent: ecspresso
---

# appspec

`appspec`コマンドは、AWS CodeDeployのAppSpec YAMLをSTDOUTに出力します。このコマンドは、Blue/Greenデプロイメントを設定する際に役立ちます。

## 構文

```
ecspresso appspec [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
| `--task-definition` | 使用するタスク定義のARN | 最新のタスク定義 |
| `--revision` | 使用するタスク定義のリビジョン | 最新のリビジョン |
| `--format` | 出力形式（yaml/json） | `yaml` |

## 使用例

### 基本的な使用方法

```bash
ecspresso appspec > appspec.yaml
```

### JSON形式での出力

```bash
ecspresso appspec --format json > appspec.json
```

### 特定のタスク定義リビジョンを使用

```bash
ecspresso appspec --revision 10 > appspec.yaml
```

## 出力例

```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/your-task-definition:10"
        LoadBalancerInfo:
          ContainerName: "web"
          ContainerPort: 80
        PlatformVersion: "1.4.0"
```

## Blue/Greenデプロイメントでの使用

`appspec`コマンドは、CodeDeployを使用したBlue/Greenデプロイメントを設定する際に使用します。ecspresso.ymlにCodeDeployの設定を追加した後、このコマンドを使用してAppSpecファイルを生成できます。

```yaml
# ecspresso.yml
codedeploy:
  application_name: AppECS-your-cluster-your-service
  deployment_group_name: DgpECS-your-cluster-your-service
  deployment_config_name: CodeDeployDefault.ECSAllAtOnce
```

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [render](./render.html) - 設定、サービス定義、またはタスク定義ファイルをSTDOUTに出力
