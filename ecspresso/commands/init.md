---
layout: default
title: init
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 1
---

# init

`init`コマンドは、既存のECSサービスから設定ファイルを生成するためのコマンドです。ecspressoを使い始める際の最初のステップとして使用します。

## 基本的な使い方

```console
$ ecspresso init [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--region` | AWSリージョン | 環境変数`AWS_REGION` |
| `--cluster` | ECSクラスター名 | - |
| `--service` | ECSサービス名 | - |
| `--config` | 生成する設定ファイルのパス | `ecspresso.yml` |
| `--task-definition` | 生成するタスク定義ファイルのパス | `ecs-task-def.json` |
| `--service-definition` | 生成するサービス定義ファイルのパス | `ecs-service-def.json` |
| `--appspec` | 生成するAppSpecファイルのパス | - |
| `--skip-task-definition` | タスク定義ファイルの生成をスキップ | `false` |
| `--skip-service-definition` | サービス定義ファイルの生成をスキップ | `false` |
| `--update-task-definition` | 既存のタスク定義ファイルを更新 | `false` |
| `--update-service-definition` | 既存のサービス定義ファイルを更新 | `false` |
| `--output-format` | 出力形式（`json`または`jsonnet`） | `json` |

## 生成されるファイル

`init`コマンドは、以下のファイルを生成します：

1. **設定ファイル** (デフォルト: `ecspresso.yml`)
   - ecspressoの設定情報

2. **タスク定義ファイル** (デフォルト: `ecs-task-def.json`)
   - ECSタスク定義の詳細情報

3. **サービス定義ファイル** (デフォルト: `ecs-service-def.json`)
   - ECSサービス定義の詳細情報

4. **AppSpecファイル** (オプション)
   - AWS CodeDeployのAppSpec情報（`--appspec`オプションを指定した場合）

## 出力例

```
2023/01/01 12:00:00 [info] myservice/default save service definition to ecs-service-def.json
2023/01/01 12:00:00 [info] myservice/default save task definition to ecs-task-def.json
2023/01/01 12:00:00 [info] myservice/default save config to ecspresso.yml
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice
```

### カスタムファイル名を指定

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice \
  --config my-config.yml \
  --task-definition my-task-def.json \
  --service-definition my-service-def.json
```

### AppSpecファイルも生成

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice \
  --appspec appspec.yaml
```

### Jsonnet形式で出力

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice \
  --output-format jsonnet
```

### 既存のファイルを更新

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice \
  --update-task-definition --update-service-definition
```

## 設定ファイルの例

`init`コマンドで生成される`ecspresso.yml`の例：

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

## 注意事項

- `--region`、`--cluster`、`--service`オプションは必須です
- 既存のファイルを上書きする場合は、`--update-task-definition`または`--update-service-definition`オプションを使用します
- 特定のファイルの生成をスキップする場合は、`--skip-task-definition`または`--skip-service-definition`オプションを使用します
- `--output-format jsonnet`を指定すると、JSONではなくJsonnet形式でファイルが生成されます
- 生成されたファイルは、必要に応じて手動で編集できます
- AWS認証情報が正しく設定されていることを確認してください

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [render](./render.html) - 設定ファイルをレンダリングして標準出力に表示
- [verify](./verify.html) - 設定ファイルの検証
