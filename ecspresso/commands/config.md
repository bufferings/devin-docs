---
layout: default
title: init, render, verify
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 3
---

# 設定関連コマンド

## init

`init`コマンドは、既存のECSサービスから構成ファイルを作成するために使用します。

### 使用方法

```
ecspresso init [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--region=REGION` | AWS リージョン | - |
| `--cluster=CLUSTER` | ECS クラスター名 | - |
| `--service=SERVICE` | ECS サービス名 | - |
| `--config=CONFIG` | 設定ファイルパス | `ecspresso.yml` |
| `--task-definition=TASK-DEF` | タスク定義ファイルパス | - |
| `--service-definition=SVC-DEF` | サービス定義ファイルパス | - |

### 使用例

```bash
$ ecspresso init --region=ap-northeast-1 --cluster=my-cluster --service=my-service
```

## render

`render`コマンドは、設定、サービス定義、またはタスク定義ファイルをSTDOUTにレンダリングするために使用します。

### 使用方法

```
ecspresso render [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--config` | 設定ファイルをレンダリングします | `false` |
| `--service-definition` | サービス定義をレンダリングします | `false` |
| `--task-definition` | タスク定義をレンダリングします | `false` |

### 使用例

設定ファイルをレンダリング:
```bash
$ ecspresso render --config
```

サービス定義をレンダリング:
```bash
$ ecspresso render --service-definition
```

タスク定義をレンダリング:
```bash
$ ecspresso render --task-definition
```

## verify

`verify`コマンドは、設定内のリソースを検証するために使用します。

### 使用方法

```
ecspresso verify [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--task-definition` | タスク定義のみを検証します | `false` |
| `--service-definition` | サービス定義のみを検証します | `false` |
| `--deployment-group` | CodeDeployのデプロイメントグループも検証します | `false` |

### 使用例

すべての設定を検証:
```bash
$ ecspresso verify
```

タスク定義のみを検証:
```bash
$ ecspresso verify --task-definition
```

サービス定義のみを検証:
```bash
$ ecspresso verify --service-definition
```

CodeDeployのデプロイメントグループも含めて検証:
```bash
$ ecspresso verify --deployment-group
```
