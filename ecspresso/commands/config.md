---
layout: default
title: 設定関連コマンド
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 3
---

# 設定関連コマンド

## init

`init`コマンドは、既存のECSサービスから設定ファイルを作成します。

```
Usage: ecspresso init [options]

Options:
  --region=REGION                 AWSリージョン
  --cluster=CLUSTER               ECSクラスター名
  --service=SERVICE               ECSサービス名
  --config=CONFIG                 設定ファイル名 (デフォルト: ecspresso.yml)
  --task-definition=ARN           タスク定義ARN
  --update-task-definition        タスク定義を更新
  --update-service-definition     サービス定義を更新
  --no-load-task-definition       タスク定義を読み込まない
  --no-load-service-definition    サービス定義を読み込まない
  --output-task-definition=FILE   タスク定義の出力先ファイル名
  --output-service-definition=FILE サービス定義の出力先ファイル名
  --force                         既存の設定ファイルを上書き
```

### 使用例

```console
# 基本的な初期化
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice

# 設定ファイル名を指定して初期化
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config myapp.yml

# タスク定義のみを読み込む
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --no-load-service-definition
```

## diff

`diff`コマンドは、現在のサービスと設定ファイルの差分を表示します。

```
Usage: ecspresso diff [options]

Options:
  --config-only                   設定ファイルのみを表示
  --task-definition=ARN           比較するタスク定義ARN
  --task-def=FILE                 比較するタスク定義ファイル
  --service-definition=ARN        比較するサービス定義ARN
  --service-def=FILE              比較するサービス定義ファイル
  --output=FORMAT                 出力形式 (text, json) (デフォルト: text)
```

### 使用例

```console
# 現在のサービスと設定ファイルの差分を表示
$ ecspresso diff

# JSON形式で差分を表示
$ ecspresso diff --output=json

# 特定のタスク定義ファイルと比較
$ ecspresso diff --task-def=new-task-def.json
```

## render

`render`コマンドは、タスク定義とサービス定義をレンダリングします。

```
Usage: ecspresso render [options]

Options:
  --task-def=FILE                 レンダリングするタスク定義ファイル
  --service-def=FILE              レンダリングするサービス定義ファイル
```

### 使用例

```console
# タスク定義とサービス定義をレンダリング
$ ecspresso render

# 特定のタスク定義ファイルをレンダリング
$ ecspresso render --task-def=my-task-def.json
```

## verify

`verify`コマンドは、タスク定義とサービス定義を検証します。

```
Usage: ecspresso verify [options]

Options:
  --task-def=FILE                 検証するタスク定義ファイル
  --service-def=FILE              検証するサービス定義ファイル
```

### 使用例

```console
# タスク定義とサービス定義を検証
$ ecspresso verify

# 特定のタスク定義ファイルを検証
$ ecspresso verify --task-def=my-task-def.json
```

## appspec

`appspec`コマンドは、CodeDeployのAppSpecファイルを生成します。

```
Usage: ecspresso appspec [options]

Options:
  --task-def=FILE                 タスク定義ファイル
  --output=FILE                   出力先ファイル名
```

### 使用例

```console
# AppSpecファイルを生成
$ ecspresso appspec

# 出力先を指定してAppSpecファイルを生成
$ ecspresso appspec --output=appspec.yaml
```

## config

`config`コマンドは、現在の設定を表示します。

```
Usage: ecspresso config [options]

Options:
  --output=FORMAT                 出力形式 (text, json) (デフォルト: text)
```

### 使用例

```console
# 現在の設定を表示
$ ecspresso config

# JSON形式で設定を表示
$ ecspresso config --output=json
```

## version

`version`コマンドは、ecspressoのバージョンを表示します。

```
Usage: ecspresso version
```

### 使用例

```console
# バージョンを表示
$ ecspresso version
```

## 設定ファイル構造

ecspressoの設定ファイル（`ecspresso.yml`）の基本構造は以下の通りです：

```yaml
region: ap-northeast-1
cluster: default
service: myservice
task_definition: ecs-task-def.json
service_definition: ecs-service-def.json
timeout: 10m
plugins:
  - name: tfstate
    config:
      path: terraform.tfstate
```

### 主な設定項目

- **region**: AWSリージョン
- **cluster**: ECSクラスター名
- **service**: ECSサービス名
- **task_definition**: タスク定義ファイルのパス
- **service_definition**: サービス定義ファイルのパス
- **timeout**: タイムアウト時間
- **plugins**: 使用するプラグインの設定
