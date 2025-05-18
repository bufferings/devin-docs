---
layout: default
title: tasks
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 2
---

# tasks

`tasks`コマンドは、サービス内のタスクまたは同じファミリーを持つタスクを一覧表示するために使用します。

## 使用方法

```
ecspresso tasks [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--id=ID` | タスクID | - |
| `--output=FORMAT` | 出力形式 (table,json,tsv) | `table` |
| `--find` | タスクリストからタスクを検索し、JSONとしてダンプします | `false` |

## 使用例

サービス内のすべてのタスクを一覧表示:
```bash
$ ecspresso tasks
```

特定のタスクの詳細を表示:
```bash
$ ecspresso tasks --id=arn:aws:ecs:ap-northeast-1:123456789012:task/cluster-name/abcdef0123456789
```

JSON形式で出力:
```bash
$ ecspresso tasks --output=json
```

タスクリストからタスクを検索してJSONとしてダンプ:
```bash
$ ecspresso tasks --find
```

## タスク情報の内容

タスク情報には以下が含まれます:

- タスクARN
- タスクの状態（RUNNING, STOPPED, etc）
- 開始時刻と停止時刻（該当する場合）
- コンテナインスタンス
- コンテナの状態
- ネットワーク構成
