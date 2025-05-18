---
layout: default
title: appspec
parent: コマンドリファレンス
nav_order: 1
---

# appspec

`appspec`コマンドは、AWS CodeDeployのAppSpec YAMLを標準出力に出力します。Blue/Greenデプロイメントを行う際に使用します。

## 使い方

```console
$ ecspresso appspec --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--base-task-definition` | ベースとなるタスク定義のJSONファイルパス |

## 出力例

```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:10"
        LoadBalancerInfo:
          ContainerName: "web"
          ContainerPort: 80
```

## 使用例

CodeDeployを使用したBlue/GreenデプロイメントのためのAppSpecファイルを生成します。

```console
$ ecspresso appspec --config ecspresso.yml > appspec.yaml
```

このコマンドは、現在のタスク定義とサービス定義から、CodeDeployで使用するためのAppSpecファイルを生成し、`appspec.yaml`ファイルに保存します。
