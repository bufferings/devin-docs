---
layout: default
title: 監視・分析コマンド
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 4
---

# 監視・分析コマンド

## status

サービスの現在の状態を表示します。

```
Usage: ecspresso status [flags]

Flags:
  --events=N            表示するイベント数（デフォルト：10）
  --tasks               タスク一覧も表示
  --quiet               簡易出力
  --wait-until=STATUS   指定ステータスになるまで待機（デフォルト：NONE）
```

### 例

サービスステータスの表示：
```console
$ ecspresso status
```

タスク一覧も含めて表示：
```console
$ ecspresso status --tasks
```

## diff

タスク定義、サービス定義と現在実行中のサービスおよびタスク定義との差分を表示します。

```
Usage: ecspresso diff [flags]

Flags:
  --[no-]unified        統一差分フォーマット（デフォルト：true）
  --external=COMMAND    差分フォーマット用の外部コマンド
```

### 例

差分の表示：
```console
$ ecspresso diff
```

外部コマンドを使用した差分表示：
```console
$ ecspresso diff --external="colordiff -u"
```

## wait

サービスが安定するまで待機します。

```
Usage: ecspresso wait [flags]

Flags:
  --timeout=DURATION    タイムアウト時間
  --service-name=NAME   サービス名（設定ファイルと異なる場合）
```

### 例

サービスが安定するまで待機：
```console
$ ecspresso wait
```

特定のサービスが安定するまで待機：
```console
$ ecspresso wait --service-name=myservice
```

## revisions

タスク定義のリビジョン履歴を表示します。

```
Usage: ecspresso revisions [flags]

Flags:
  --output=FORMAT       出力フォーマット（table, json, tsv）（デフォルト：table）
  --max-items=N         表示する最大項目数（デフォルト：100）
```

### 例

リビジョン履歴の表示：
```console
$ ecspresso revisions
```

JSON形式でリビジョン履歴を表示：
```console
$ ecspresso revisions --output=json
```
