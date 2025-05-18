---
layout: default
title: deregister
parent: コマンドリファレンス
nav_order: 4
---

# deregister

`deregister`コマンドは、タスク定義を登録解除（deregister）します。

## 使い方

```console
$ ecspresso deregister --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--revision` | 登録解除するタスク定義のリビジョン |
| `--dry-run` | 実際に登録解除を行わずに実行内容を表示 |

## 使用例

### 現在の設定ファイルに基づいたタスク定義を登録解除

```console
$ ecspresso deregister --config ecspresso.yml
```

### 特定のリビジョンのタスク定義を登録解除

```console
$ ecspresso deregister --config ecspresso.yml --revision 10
```

### ドライランモードで実行内容を確認

```console
$ ecspresso deregister --config ecspresso.yml --dry-run
```

## 注意事項

- 登録解除されたタスク定義は、新しいタスクの起動に使用できなくなります。
- 既に実行中のタスクには影響しません。
- 登録解除されたタスク定義は、AWS ECSコンソールでは「INACTIVE」と表示されます。
- 登録解除は元に戻せないため、必要に応じて事前にタスク定義のバックアップを取ることをお勧めします。
