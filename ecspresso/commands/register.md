---
layout: default
title: register
parent: コマンドリファレンス
nav_order: 9
---

# register

`register`コマンドは、タスク定義を登録します。

## 使い方

```console
$ ecspresso register --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--dry-run` | 実際に登録を行わずに実行内容を表示 |

## 使用例

### 基本的な使用方法

```console
$ ecspresso register --config ecspresso.yml
```

### 特定のタスク定義ファイルを指定

```console
$ ecspresso register --config ecspresso.yml --task-definition my-task-def.json
```

### ドライランモードで実行内容を確認

```console
$ ecspresso register --config ecspresso.yml --dry-run
```

## 出力例

```
2019/10/15 22:47:08 myService/default Creating a new task definition by ecs-task-def.json
2019/10/15 22:47:08 myService/default Registering a new task definition...
2019/10/15 22:47:08 myService/default Task definition is registered myService:6
```

## 注意事項

- このコマンドはタスク定義のみを登録し、サービスの更新は行いません。
- サービスを更新するには、`deploy`コマンドを使用してください。
- 登録されたタスク定義は、AWS ECSコンソールで確認できます。
