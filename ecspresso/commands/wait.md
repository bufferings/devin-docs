---
layout: default
title: wait
parent: コマンドリファレンス
nav_order: 19
---

# wait

`wait`コマンドは、サービスが安定するまで待機します。

## 使い方

```console
$ ecspresso wait --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--timeout` | タイムアウト時間（デフォルト: 10m） |

## 使用例

### 基本的な使用方法

```console
$ ecspresso wait --config ecspresso.yml
```

### タイムアウト時間を指定

```console
$ ecspresso wait --config ecspresso.yml --timeout 5m
```

## 出力例

```
2023/04/01 12:34:56 myService/default Waiting for service stable...
2023/04/01 12:35:56 myService/default  PRIMARY myService:4 desired:2 pending:0 running:2
2023/04/01 12:36:56 myService/default Service is stable now. Completed!
```

## 使用シナリオ

`wait`コマンドは、以下のような場合に便利です：

1. デプロイ後にサービスが安定するまで待機したい場合
2. スクリプトやCI/CDパイプラインでデプロイの完了を確認したい場合
3. サービスの更新が完了したことを確認してから次のステップに進みたい場合

## 注意事項

- このコマンドはサービスが安定するまで待機するだけで、変更は行いません。
- サービスが安定するとは、デザイアドカウントとランニングカウントが一致し、保留中のタスクがない状態を指します。
- タイムアウト時間を超えても安定しない場合、エラーが発生します。
