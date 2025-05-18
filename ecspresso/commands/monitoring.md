---
layout: default
title: status, wait, exec
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 4
---

# モニタリング関連コマンド

## status

`status`コマンドは、サービスのステータスを表示するために使用します。

### 使用方法

```
ecspresso status [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--events=N` | 表示するイベント数 | `10` |

### 使用例

```bash
$ ecspresso status
```

イベント数を変更して表示:
```bash
$ ecspresso status --events=20
```

## wait

`wait`コマンドは、サービスが安定するまで待機するために使用します。

### 使用方法

```
ecspresso wait [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--timeout=DURATION` | タイムアウト | - |
| `--wait-until=STATE` | 待機する状態 (stable,deployed) | `stable` |

### 使用例

サービスが安定するまで待機:
```bash
$ ecspresso wait
```

5分のタイムアウトを設定して待機:
```bash
$ ecspresso wait --timeout=5m
```

デプロイが完了するまで待機:
```bash
$ ecspresso wait --wait-until=deployed
```

## exec

`exec`コマンドは、タスク上でコマンドを実行するために使用します。

### 使用方法

```
ecspresso exec [オプション] -- COMMAND...
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--id=ID` | タスクID | - |
| `--container=CONTAINER` | コンテナ名 | - |
| `--dry-run` | 実行せずに表示 | `false` |

### 使用例

最新のタスクでコマンドを実行:
```bash
$ ecspresso exec -- ls -la
```

特定のタスクでコマンドを実行:
```bash
$ ecspresso exec --id=arn:aws:ecs:ap-northeast-1:123456789012:task/cluster-name/abcdef0123456789 -- ps aux
```

特定のコンテナでコマンドを実行:
```bash
$ ecspresso exec --container=nginx -- nginx -t
```
