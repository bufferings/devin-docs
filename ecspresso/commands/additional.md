---
layout: default
title: run, register, appspec, その他
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 5
---

# その他のコマンド

## run

`run`コマンドは、タスクを実行するために使用します。

### 使用方法

```
ecspresso run [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--dry-run` | 実行せずに表示 | `false` |
| `--task-def=TASK-DEF` | 実行するタスク定義ファイル | - |
| `--[no-]wait` | タスクの完了を待機 | `true` |
| `--overrides=JSON` | タスク定義のオーバーライド (JSON形式) | - |
| `--count=N` | 実行するタスクの数 | `1` |
| `--launch-type=TYPE` | 起動タイプ (EC2,FARGATE) | - |
| `--network-configuration=JSON` | ネットワーク設定 (JSON形式) | - |
| `--cluster=CLUSTER` | ECSクラスター名 | 設定ファイルの値 |
| `--platform-version=VERSION` | プラットフォームバージョン | - |
| `--group=GROUP` | タスクグループ | - |
| `--container-name=NAME` | --commands引数用のコンテナ名 | - |
| `--commands=COMMANDS` | コンテナに渡すコマンド (カンマ区切り) | - |

### 使用例

基本的なタスク実行:
```bash
$ ecspresso run
```

特定のタスク定義ファイルを使用:
```bash
$ ecspresso run --task-def=custom-task-def.json
```

タスク定義をオーバーライド:
```bash
$ ecspresso run --overrides='{"containerOverrides":[{"name":"app","command":["echo","hello"]}]}'
```

複数のタスクを実行:
```bash
$ ecspresso run --count=3
```

## register

`register`コマンドは、タスク定義を登録するために使用します。

### 使用方法

```
ecspresso register [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--dry-run` | 実行せずに表示 | `false` |
| `--latest-task-definition` | 最新のタスク定義を使用 | `false` |

### 使用例

タスク定義を登録:
```bash
$ ecspresso register
```

ドライランで表示:
```bash
$ ecspresso register --dry-run
```

## appspec

`appspec`コマンドは、CodeDeployのAppSpecをSTDOUTに出力するために使用します。

### 使用方法

```
ecspresso appspec [オプション]
```

### オプション

| オプション | 説明 | デフォルト値 |
|------------|------|--------------|
| `--task-definition=ARN` | タスク定義ARN | - |
| `--revision=N` | タスク定義のリビジョン | `0` |

### 使用例

AppSpecを出力:
```bash
$ ecspresso appspec
```

特定のタスク定義ARNを使用:
```bash
$ ecspresso appspec --task-definition=arn:aws:ecs:ap-northeast-1:123456789012:task-definition/my-task:1
```

## その他のコマンド

### delete

サービスを削除します:
```bash
$ ecspresso delete
```

### deregister

タスク定義を登録解除します:
```bash
$ ecspresso deregister
```

### diff

タスク定義、サービス定義と現在実行中のサービスとタスク定義の差分を表示します:
```bash
$ ecspresso diff
```

### refresh

サービスを更新します（deploy --skip-task-definition --force-new-deployment --no-update-serviceと同等）:
```bash
$ ecspresso refresh
```

### revisions

タスク定義のリビジョンを表示します:
```bash
$ ecspresso revisions
```

### rollback

サービスをロールバックします:
```bash
$ ecspresso rollback
```

### scale

サービスをスケールします（deploy --skip-task-definition --no-update-serviceと同等）:
```bash
$ ecspresso scale --tasks=5
```
