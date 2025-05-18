---
layout: default
title: init
parent: コマンドリファレンス
nav_order: 7
---

# init

`init`コマンドは、既存のECSサービスから設定ファイルを作成します。これはecspressoを使い始める際の最初のステップです。

## 使い方

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--region` | AWSリージョン |
| `--cluster` | ECSクラスター名 |
| `--service` | ECSサービス名 |
| `--config` | 作成する設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | 作成するタスク定義のJSONファイルパス（デフォルト: ecs-task-def.json） |
| `--service-definition` | 作成するサービス定義のJSONファイルパス（デフォルト: ecs-service-def.json） |
| `--appspec-definition` | 作成するAppSpec定義のJSONファイルパス |
| `--skip-task-definition` | タスク定義の作成をスキップ |
| `--skip-service-definition` | サービス定義の作成をスキップ |
| `--no-load-balancer` | ロードバランサー設定を含めない |
| `--format` | 出力形式（json, yaml, jsonnet）（デフォルト: json） |

## 使用例

### 基本的な使用方法

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
2019/10/12 01:31:48 myservice/default save service definition to ecs-service-def.json
2019/10/12 01:31:48 myservice/default save task definition to ecs-task-def.json
2019/10/12 01:31:48 myservice/default save config to ecspresso.yml
```

### YAML形式で出力

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml --format yaml
```

### Jsonnet形式で出力

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml --format jsonnet
```

### ロードバランサー設定を含めない

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml --no-load-balancer
```

## 生成されるファイル

### ecspresso.yml

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

### ecs-service-def.json

サービス定義のJSONファイルが生成されます。

### ecs-task-def.json

タスク定義のJSONファイルが生成されます。

## 注意事項

- 既存のファイルがある場合は上書きされます。
- 生成されたファイルは、必要に応じて編集できます。
- 設定ファイルは、YAML、JSON、Jsonnet形式で出力できます。
