---
layout: default
title: revisions
parent: コマンドリファレンス
nav_order: 11
---

# revisions

`revisions`コマンドは、タスク定義のリビジョンを表示します。

## 使い方

```console
$ ecspresso revisions --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--max-rows` | 表示する最大行数（デフォルト: 100） |

## 使用例

### 基本的な使用方法

```console
$ ecspresso revisions --config ecspresso.yml
```

### 表示する最大行数を指定

```console
$ ecspresso revisions --config ecspresso.yml --max-rows 10
```

## 出力例

```
FAMILY  REVISION        REGISTERED AT           STATUS
myapp   21      2023-04-01 12:34:56 +0900 JST   ACTIVE
myapp   20      2023-03-30 15:45:12 +0900 JST   ACTIVE
myapp   19      2023-03-25 09:12:34 +0900 JST   ACTIVE
myapp   18      2023-03-20 18:23:45 +0900 JST   INACTIVE
myapp   17      2023-03-15 14:56:23 +0900 JST   INACTIVE
```

## 使用シナリオ

`revisions`コマンドは、以下のような場合に便利です：

1. 過去のデプロイ履歴を確認したい場合
2. ロールバック先のリビジョンを選択したい場合
3. 不要なリビジョンを特定して登録解除したい場合

## 注意事項

- このコマンドはタスク定義のリビジョンを表示するだけで、変更は行いません。
- デフォルトでは最新の100リビジョンが表示されます。
- `ACTIVE`ステータスのタスク定義は、新しいタスクの起動に使用できます。
- `INACTIVE`ステータスのタスク定義は、登録解除されたものです。
