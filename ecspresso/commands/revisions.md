---
layout: default
title: revisions
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 15
---

# revisions

`revisions`コマンドは、タスク定義のリビジョンを一覧表示します。

## 基本的な使い方

```bash
ecspresso revisions --config CONFIG_FILE
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--output` | 出力形式（table, json, yaml） | `table` |
| `--max-items` | 表示するリビジョンの最大数 | `100` |

## 詳細

`revisions`コマンドは、以下の処理を行います：

1. 設定ファイルからタスク定義情報を読み込む
2. タスク定義のリビジョンを一覧表示する

このコマンドは、タスク定義のリビジョン履歴を確認するのに役立ちます。

## 出力例

### テーブル形式（デフォルト）

```
REVISION    REGISTERED AT                     STATUS
20          2023-01-01 00:00:00 +0000 UTC    ACTIVE
19          2022-12-31 00:00:00 +0000 UTC    ACTIVE
18          2022-12-30 00:00:00 +0000 UTC    ACTIVE
17          2022-12-29 00:00:00 +0000 UTC    ACTIVE
16          2022-12-28 00:00:00 +0000 UTC    ACTIVE
```

### JSON形式

```json
[
  {
    "revision": 20,
    "registeredAt": "2023-01-01T00:00:00Z",
    "status": "ACTIVE"
  },
  {
    "revision": 19,
    "registeredAt": "2022-12-31T00:00:00Z",
    "status": "ACTIVE"
  },
  {
    "revision": 18,
    "registeredAt": "2022-12-30T00:00:00Z",
    "status": "ACTIVE"
  },
  {
    "revision": 17,
    "registeredAt": "2022-12-29T00:00:00Z",
    "status": "ACTIVE"
  },
  {
    "revision": 16,
    "registeredAt": "2022-12-28T00:00:00Z",
    "status": "ACTIVE"
  }
]
```

## 使用例

### 基本的な使用例

```bash
ecspresso revisions --config ecspresso.yml
```

### JSON形式で出力する例

```bash
ecspresso revisions --config ecspresso.yml --output json
```

### YAML形式で出力する例

```bash
ecspresso revisions --config ecspresso.yml --output yaml
```

### 表示するリビジョンの最大数を指定する例

```bash
ecspresso revisions --config ecspresso.yml --max-items 10
```

## タスク定義のリビジョン

タスク定義のリビジョンは、タスク定義のバージョンを表す番号です。新しいタスク定義を登録するたびに、リビジョン番号が増加します。

例えば、タスク定義ファミリー「myapp」のリビジョン10のARNは「arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10」です。

## タスク定義のステータス

タスク定義のステータスには、以下の2種類があります：

- `ACTIVE` - アクティブなタスク定義。新しいタスクを起動できます。
- `INACTIVE` - 登録解除されたタスク定義。新しいタスクを起動できません。

## 関連コマンド

- `register` - タスク定義を登録します。
- `deregister` - タスク定義を登録解除します。
- `deploy` - タスク定義を登録し、サービスを更新します。
- `rollback` - サービスを以前のタスク定義にロールバックします。
