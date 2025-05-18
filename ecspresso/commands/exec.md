---
layout: default
title: exec
parent: コマンドリファレンス
nav_order: 6
---

# exec

`exec`コマンドは、実行中のECSタスク上でコマンドを実行します。SSHのように、実行中のコンテナに接続してコマンドを実行できます。

## 使い方

```console
$ ecspresso exec --config ecspresso.yml --command /bin/bash
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--command` | 実行するコマンド |
| `--container` | コマンドを実行するコンテナ名 |
| `--task-id` | コマンドを実行するタスクID |
| `--port-forward` | ポートフォワーディングを有効にする |
| `--port` | ポートフォワーディングするコンテナのポート |
| `--local-port` | ポートフォワーディングするローカルのポート |
| `--interactive` | 対話モードを有効にする（デフォルト: true） |
| `--no-interactive` | 対話モードを無効にする |
| `--timeout` | タイムアウト時間 |

## 使用例

### 基本的な使用方法（対話モード）

```console
$ ecspresso exec --config ecspresso.yml --command /bin/bash
```

### 特定のコンテナでコマンドを実行

```console
$ ecspresso exec --config ecspresso.yml --command "ls -la" --container app
```

### 特定のタスクでコマンドを実行

```console
$ ecspresso exec --config ecspresso.yml --command "ps aux" --task-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### ポートフォワーディングを使用

```console
$ ecspresso exec --config ecspresso.yml --port-forward --port 80 --local-port 8080
```

これにより、コンテナの80番ポートがローカルの8080番ポートにフォワーディングされます。

## 注意事項

- このコマンドを使用するには、ECSタスクでECS Execが有効になっている必要があります。
- タスク実行ロールに適切な権限（`ssmmessages:CreateControlChannel`、`ssmmessages:CreateDataChannel`、`ssmmessages:OpenControlChannel`、`ssmmessages:OpenDataChannel`）が必要です。
- タスク定義で`enableExecuteCommand`が`true`に設定されている必要があります。
- ポートフォワーディングを使用する場合、指定したポートが既に使用されていないことを確認してください。
