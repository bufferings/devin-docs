---
layout: default
title: tasks
parent: コマンドリファレンス
nav_order: 16
---

# tasks

`tasks`コマンドは、サービス内または同じファミリーを持つタスクをリストします。

## 使い方

```console
$ ecspresso tasks --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--id-only` | タスクIDのみを表示 |
| `--output` | 出力形式（table、json、tsv）（デフォルト: table） |
| `--find-stopped` | 停止したタスクも表示 |
| `--status` | 表示するタスクのステータス（RUNNING、STOPPED、ALL）（デフォルト: RUNNING） |

## 使用例

### 基本的な使用方法

```console
$ ecspresso tasks --config ecspresso.yml
```

### タスクIDのみを表示

```console
$ ecspresso tasks --config ecspresso.yml --id-only
```

### JSON形式で出力

```console
$ ecspresso tasks --config ecspresso.yml --output json
```

### 停止したタスクも表示

```console
$ ecspresso tasks --config ecspresso.yml --find-stopped
```

### 特定のステータスのタスクを表示

```console
$ ecspresso tasks --config ecspresso.yml --status STOPPED
```

## 出力例

```
ID                                          STATUS    TASK DEFINITION                                                 STARTED     STOPPED
12345678-1234-1234-1234-123456789012        RUNNING   arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:3  2h ago      -
23456789-2345-2345-2345-234567890123        RUNNING   arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:3  30m ago     -
```

## 使用シナリオ

`tasks`コマンドは、以下のような場合に便利です：

1. 実行中のタスクを確認したい場合
2. タスクIDを取得して`exec`コマンドで接続したい場合
3. 停止したタスクの情報を確認したい場合
4. タスクの実行時間を確認したい場合

## 注意事項

- このコマンドはタスクの一覧を表示するだけで、変更は行いません。
- デフォルトでは、実行中のタスクのみが表示されます。
- `--find-stopped`オプションを使用すると、停止したタスクも表示されます。
