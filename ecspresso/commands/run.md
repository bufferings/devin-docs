---
layout: default
title: run
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 5
---

# run

`run`コマンドは、一時的なECSタスクを実行するためのコマンドです。データベースマイグレーションやバッチ処理などの一時的な作業に役立ちます。

## 基本的な使い方

```console
$ ecspresso run [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--dry-run` | 実際に実行せずに実行内容を表示 | `false` |
| `--task-def` | 実行するタスク定義ファイル | 設定ファイルのタスク定義 |
| `--wait` | タスクの完了を待機 | `true` |
| `--no-wait` | タスクの完了を待機しない | `false` |
| `--overrides` | タスクオーバーライドのJSON文字列 | - |
| `--overrides-file` | タスクオーバーライドのJSONファイルパス | - |
| `--skip-task-definition` | 新しいタスク定義の登録をスキップ | `false` |
| `--count` | 実行するタスクの数（最大10） | `1` |
| `--watch-container` | 終了コードを監視するコンテナ名 | - |
| `--latest-task-definition` | 最新のタスク定義を使用 | `false` |
| `--propagate-tags` | タスクにタグを伝播（`SERVICE`または`TASK_DEFINITION`） | - |
| `--tags` | タスクのタグ（形式：`KeyFoo=ValueFoo,KeyBar=ValueBar`） | - |
| `--wait-until` | タスクの状態が指定された状態になるまで待機（`running`または`stopped`） | `stopped` |
| `--revision` | `--skip-task-definition`使用時に実行するタスク定義のリビジョン | - |
| `--client-token` | リクエストを識別する一意のトークン（べき等性に役立つ） | - |
| `--ebs-delete-on-termination` | タスク停止時にEBSボリュームを削除するかどうか | `true` |
| `--no-ebs-delete-on-termination` | タスク停止時にEBSボリュームを削除しない | `false` |

## 実行プロセス

`run`コマンドは、以下のプロセスでタスクを実行します：

1. タスク定義ファイルから新しいタスク定義を登録（`--skip-task-definition`または`--latest-task-definition`が指定されていない場合）
2. 指定されたオーバーライドを適用してタスクを実行
3. タスクの完了を待機（`--no-wait`が指定されていない場合）
4. ログを表示（CloudWatch Logsが設定されている場合）

```mermaid
graph TD
    A[開始] --> B{タスク定義の更新をスキップ?}
    B -->|はい| D[タスクを実行]
    B -->|いいえ| C[タスク定義の登録]
    C --> D
    D --> E{待機をスキップ?}
    E -->|はい| G[完了]
    E -->|いいえ| F[タスクの完了を待機]
    F --> G
```

## 出力例

### 通常実行

```
2023/01/01 12:00:00 [info] myservice/default Running task
2023/01/01 12:00:00 [info] myservice/default Task definition family myservice will be registered
2023/01/01 12:00:00 [info] myservice/default Watch container: web
2023/01/01 12:00:00 [info] myservice/default Running task with arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:4
2023/01/01 12:00:00 [info] myservice/default Task ARN: arn:aws:ecs:ap-northeast-1:123456789012:task/default/12345678-1234-1234-1234-123456789012
2023/01/01 12:00:00 [info] myservice/default Waiting for run task...(it may take a while)
2023/01/01 12:00:00 [info] myservice/default Watching container: web
2023/01/01 12:00:00 [info] myservice/default Waiting for task ID 12345678-1234-1234-1234-123456789012 until stopped
2023/01/01 12:00:10 [info] myservice/default 2023/01/01 12:00:10 Starting application...
2023/01/01 12:00:20 [info] myservice/default 2023/01/01 12:00:20 Application completed successfully!
2023/01/01 12:00:30 [info] myservice/default Run task completed!
```

### ドライラン

```
2023/01/01 12:00:00 [info] myservice/default Running task
2023/01/01 12:00:00 [info] myservice/default Task definition family myservice will be registered
2023/01/01 12:00:00 [info] myservice/default Watch container: web
2023/01/01 12:00:00 [info] myservice/default DRY RUN OK
```

## 使用例

### 基本的なタスク実行

```console
$ ecspresso run --config ecspresso.yml
```

### カスタムタスク定義ファイルを使用

```console
$ ecspresso run --config ecspresso.yml --task-def custom-task-def.json
```

### コマンドのオーバーライド

```console
$ ecspresso run --config ecspresso.yml --overrides '{"containerOverrides":[{"name":"web","command":["./migrate.sh"]}]}'
```

### オーバーライドファイルを使用

```console
$ cat overrides.json
{
  "containerOverrides": [
    {
      "name": "web",
      "command": ["./migrate.sh"]
    }
  ]
}

$ ecspresso run --config ecspresso.yml --overrides-file overrides.json
```

### 複数のタスクを実行

```console
$ ecspresso run --config ecspresso.yml --count 3
```

### 特定のコンテナを監視

```console
$ ecspresso run --config ecspresso.yml --watch-container worker
```

### タスクが実行状態になるまで待機

```console
$ ecspresso run --config ecspresso.yml --wait-until running
```

### 最新のタスク定義を使用

```console
$ ecspresso run --config ecspresso.yml --latest-task-definition
```

### タグを指定

```console
$ ecspresso run --config ecspresso.yml --tags "Environment=production,Project=myapp"
```

### サービスからタグを伝播

```console
$ ecspresso run --config ecspresso.yml --propagate-tags SERVICE
```

### EBSボリュームを保持

```console
$ ecspresso run --config ecspresso.yml --no-ebs-delete-on-termination
```

## タスクオーバーライド

タスクオーバーライドを使用すると、タスク定義を変更せずにタスクの実行時に設定を上書きできます。主な用途は以下の通りです：

1. **コマンドの変更**
   - コンテナのコマンドを上書き

   ```json
   {
     "containerOverrides": [
       {
         "name": "web",
         "command": ["./migrate.sh", "--env", "production"]
       }
     ]
   }
   ```

2. **環境変数の追加または変更**
   - コンテナの環境変数を上書き

   ```json
   {
     "containerOverrides": [
       {
         "name": "web",
         "environment": [
           {
             "name": "DATABASE_URL",
             "value": "postgres://user:password@host:5432/db"
           }
         ]
       }
     ]
   }
   ```

3. **リソース制限の変更**
   - CPUやメモリの制限を上書き

   ```json
   {
     "containerOverrides": [
       {
         "name": "web",
         "cpu": 1024,
         "memory": 2048
       }
     ]
   }
   ```

## ログの表示

`run`コマンドは、タスクのログをリアルタイムで表示します。これには、以下の条件が必要です：

1. タスク定義でCloudWatch Logsが設定されていること
2. `awslogs-stream-prefix`が設定されていること

```json
"logConfiguration": {
  "logDriver": "awslogs",
  "options": {
    "awslogs-group": "/ecs/myservice",
    "awslogs-region": "ap-northeast-1",
    "awslogs-stream-prefix": "web"
  }
}
```

## 注意事項

- `--dry-run`オプションを使用すると、実際の変更を適用せずに実行内容を確認できます
- `--no-wait`オプションを使用すると、タスクの完了を待機せずに即座に制御が戻ります
- `--count`オプションで複数のタスクを実行できますが、最大数は10です
- `--watch-container`オプションを使用すると、特定のコンテナの終了コードを監視できます
- `--wait-until running`オプションを使用すると、タスクが実行状態になった時点で待機が終了します
- `--latest-task-definition`オプションを使用すると、最新のタスク定義を使用してタスクを実行できます
- `--propagate-tags`オプションを使用すると、サービスまたはタスク定義からタグを伝播できます
- `--ebs-delete-on-termination`オプションを使用すると、タスク停止時にEBSボリュームを削除するかどうかを制御できます
- タスクオーバーライドを使用すると、タスク定義を変更せずにタスクの実行時に設定を上書きできます

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [status](./status.html) - サービスの状態を表示
- [tasks](./tasks.html) - タスクの一覧を表示
- [exec](./exec.html) - タスク内でコマンドを実行
