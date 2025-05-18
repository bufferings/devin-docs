---
layout: default
title: status
parent: コマンドリファレンス
nav_order: 15
---

# status

`status`コマンドは、ECSサービスの現在のステータスを表示します。

## 使い方

```console
$ ecspresso status --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--events` | サービスイベントを表示 |

## 使用例

### 基本的な使用方法

```console
$ ecspresso status --config ecspresso.yml
```

### サービスイベントを表示

```console
$ ecspresso status --config ecspresso.yml --events
```

## 出力例

```
Service: myService
Cluster: default
TaskDefinition: myService:3
Deployments:
    PRIMARY myService:3 desired:1 pending:0 running:1
Events:
    2017/11/09 23:20:13 (service myService) has reached a steady state.
    2017/11/09 23:20:13 (service myService) registered 1 targets in target-group tf-myService-01234567
```

## 使用シナリオ

`status`コマンドは、以下のような場合に便利です：

1. サービスの現在の状態を確認したい場合
2. デプロイメントの進行状況を確認したい場合
3. サービスに関連するイベントを確認したい場合
4. タスクの実行状況を確認したい場合

## 注意事項

- このコマンドはサービスの状態を表示するだけで、変更は行いません。
- `--events`オプションを使用すると、最近のサービスイベントが表示されます。
