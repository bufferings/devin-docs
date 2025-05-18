---
layout: default
title: tasks
parent: コマンドリファレンス
nav_order: 15
---

# tasks

`tasks`コマンドは、ECSサービスで実行中のタスク一覧を表示します。タスクのステータス確認やデバッグに役立ちます。

## 基本的な使い方

```bash
ecspresso tasks
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--id-only` | タスクIDのみを表示 | `false` |
| `--status` | フィルタリングするタスクのステータス（RUNNING/STOPPED） | - |
| `--output` | 出力形式（table/json/tsv） | `table` |

## 表示される情報

`tasks`コマンドは、以下の情報を表示します：

1. **タスクID** - タスクの一意の識別子
2. **ステータス** - タスクの現在のステータス（RUNNING/STOPPED）
3. **タスク定義** - タスクが使用しているタスク定義
4. **起動時刻** - タスクが起動された時刻
5. **停止時刻** - タスクが停止された時刻（停止している場合）
6. **停止理由** - タスクが停止された理由（停止している場合）

## 出力形式

`--output`オプションで出力形式を指定できます：

### テーブル形式（デフォルト）

```
ID                                    STATUS   TASK DEFINITION                 STARTED AT          STOPPED AT
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  RUNNING  your-task-definition:3          2023-01-01 12:00:00
yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy  STOPPED  your-task-definition:2          2023-01-01 10:00:00 2023-01-01 11:00:00
```

### JSON形式

```bash
ecspresso tasks --output=json
```

```json
[
  {
    "taskArn": "arn:aws:ecs:ap-northeast-1:123456789012:task/your-cluster/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "taskDefinitionArn": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/your-task-definition:3",
    "status": "RUNNING",
    "startedAt": "2023-01-01T12:00:00Z"
  },
  {
    "taskArn": "arn:aws:ecs:ap-northeast-1:123456789012:task/your-cluster/yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
    "taskDefinitionArn": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/your-task-definition:2",
    "status": "STOPPED",
    "startedAt": "2023-01-01T10:00:00Z",
    "stoppedAt": "2023-01-01T11:00:00Z",
    "stoppedReason": "Essential container in task exited"
  }
]
```

### TSV形式

```bash
ecspresso tasks --output=tsv
```

```
ID	STATUS	TASK DEFINITION	STARTED AT	STOPPED AT
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx	RUNNING	your-task-definition:3	2023-01-01 12:00:00	
yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy	STOPPED	your-task-definition:2	2023-01-01 10:00:00	2023-01-01 11:00:00
```

## タスクIDのみを表示

`--id-only`オプションを使用すると、タスクIDのみを表示します。これは、他のコマンドへの入力として使用する場合に便利です。

```bash
ecspresso tasks --id-only
```

```
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy
```

## ステータスでフィルタリング

`--status`オプションを使用すると、特定のステータスのタスクのみを表示します。

```bash
# 実行中のタスクのみを表示
ecspresso tasks --status=RUNNING

# 停止したタスクのみを表示
ecspresso tasks --status=STOPPED
```

## 使用例

### 基本的な使用方法

```bash
ecspresso tasks
```

### 実行中のタスクのIDのみを表示

```bash
ecspresso tasks --status=RUNNING --id-only
```

### JSON形式で出力

```bash
ecspresso tasks --output=json
```

### 他のコマンドとの組み合わせ

```bash
# 実行中の最初のタスクでコマンドを実行
TASK_ID=$(ecspresso tasks --status=RUNNING --id-only | head -n 1)
ecspresso exec --task-id=$TASK_ID --command="ls -la"
```

## 注意事項

- サービスが存在しない場合は、エラーが発生します。
- タスクが存在しない場合は、空の結果が表示されます。
- `--status`オプションで指定できるのは、`RUNNING`または`STOPPED`のみです。
