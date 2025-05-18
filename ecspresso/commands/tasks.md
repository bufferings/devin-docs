---
layout: default
title: tasks
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 16
---

# tasks

`tasks`コマンドは、ECSタスクを一覧表示します。

## 基本的な使い方

```bash
ecspresso tasks --config CONFIG_FILE
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--service` | サービス名 | 設定ファイルで指定されたサービス |
| `--cluster` | ECSクラスター名 | 設定ファイルで指定されたクラスター |
| `--status` | タスクのステータス（RUNNING, STOPPED） | - |
| `--output` | 出力形式（table, json, yaml） | `table` |
| `--find-task` | タスクIDの一部を指定して検索 | - |
| `--show-container-reasons` | コンテナの停止理由を表示するかどうか | `false` |

## 詳細

`tasks`コマンドは、以下の処理を行います：

1. 設定ファイルからサービス情報を読み込む
2. ECSタスクを一覧表示する

このコマンドは、サービスのタスク状態を確認するのに役立ちます。

## 出力例

### テーブル形式（デフォルト）

```
TASK ID                               STATUS    TASK DEFINITION                                  STARTED AT                      STOPPED AT
1234567890abcdef0123456789abcdef     RUNNING   arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10   2023-01-01 00:00:00 +0000 UTC    -
2345678901bcdef0123456789abcdef0     RUNNING   arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10   2023-01-01 00:00:00 +0000 UTC    -
3456789012cdef0123456789abcdef01     STOPPED   arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:9    2022-12-31 00:00:00 +0000 UTC    2023-01-01 00:00:00 +0000 UTC
```

### JSON形式

```json
[
  {
    "taskId": "1234567890abcdef0123456789abcdef",
    "status": "RUNNING",
    "taskDefinition": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10",
    "startedAt": "2023-01-01T00:00:00Z",
    "stoppedAt": null
  },
  {
    "taskId": "2345678901bcdef0123456789abcdef0",
    "status": "RUNNING",
    "taskDefinition": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10",
    "startedAt": "2023-01-01T00:00:00Z",
    "stoppedAt": null
  },
  {
    "taskId": "3456789012cdef0123456789abcdef01",
    "status": "STOPPED",
    "taskDefinition": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:9",
    "startedAt": "2022-12-31T00:00:00Z",
    "stoppedAt": "2023-01-01T00:00:00Z"
  }
]
```

## 使用例

### 基本的な使用例

```bash
ecspresso tasks --config ecspresso.yml
```

### 特定のサービスのタスクを表示する例

```bash
ecspresso tasks --config ecspresso.yml --service myservice
```

### 特定のクラスターのタスクを表示する例

```bash
ecspresso tasks --config ecspresso.yml --cluster mycluster
```

### 特定のステータスのタスクを表示する例

```bash
ecspresso tasks --config ecspresso.yml --status RUNNING
```

### JSON形式で出力する例

```bash
ecspresso tasks --config ecspresso.yml --output json
```

### タスクIDで検索する例

```bash
ecspresso tasks --config ecspresso.yml --find-task 1234567890
```

### コンテナの停止理由を表示する例

```bash
ecspresso tasks --config ecspresso.yml --show-container-reasons
```

## タスクのステータス

ECSタスクのステータスには、以下の種類があります：

- `PROVISIONING` - タスクのリソースをプロビジョニング中
- `PENDING` - タスクの起動準備中
- `ACTIVATING` - タスクのアクティベーション中
- `RUNNING` - タスクが実行中
- `DEACTIVATING` - タスクの非アクティブ化中
- `STOPPING` - タスクの停止中
- `DEPROVISIONING` - タスクのリソースの解放中
- `STOPPED` - タスクが停止済み

## コンテナの停止理由

`--show-container-reasons`オプションを指定すると、停止したコンテナの停止理由が表示されます。これは、タスクが予期せず停止した場合のトラブルシューティングに役立ちます。

停止理由の例：
- `Essential container in task exited` - 必須コンテナが終了した
- `Task failed ELB health checks in (elb elb-name)` - ELBのヘルスチェックに失敗した
- `Container encountered an error during StartContainer: exit status 1: ...` - コンテナの起動中にエラーが発生した

## 関連コマンド

- `status` - サービスのステータスを表示します。
- `exec` - タスク上でコマンドを実行します。
- `run` - タスクを実行します。
- `wait` - サービスが安定するまで待機します。
