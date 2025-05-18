---
layout: default
title: refresh
parent: コマンドリファレンス
nav_order: 8
---

# refresh

`refresh`コマンドは、サービスを更新します。これは`deploy --skip-task-definition --force-new-deployment --no-update-service`と同等の機能を持ちます。

## 使い方

```console
$ ecspresso refresh --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--wait-until-stable` | サービスが安定するまで待機（デフォルト: true） |
| `--no-wait-until-stable` | サービスが安定するまで待機しない |
| `--suspend-auto-scaling` | Auto Scalingを一時停止 |
| `--resume-auto-scaling` | Auto Scalingを再開 |
| `--dry-run` | 実際に更新を行わずに実行内容を表示 |

## 使用例

### 基本的な使用方法

```console
$ ecspresso refresh --config ecspresso.yml
```

### サービスが安定するまで待機しない

```console
$ ecspresso refresh --config ecspresso.yml --no-wait-until-stable
```

### Auto Scalingを再開

```console
$ ecspresso refresh --config ecspresso.yml --resume-auto-scaling
```

### ドライランモードで実行内容を確認

```console
$ ecspresso refresh --config ecspresso.yml --dry-run
```

## 使用シナリオ

`refresh`コマンドは、以下のような場合に便利です：

1. タスク定義を変更せずに、新しいタスクを強制的にデプロイしたい場合
2. 問題のあるタスクを再起動したい場合
3. Auto Scalingの設定を変更したい場合

## 注意事項

- このコマンドは新しいタスク定義を登録しません。
- サービスの設定（デザイアドカウントなど）は変更されません。
- `--force-new-deployment`フラグにより、同じタスク定義でも新しいデプロイメントが作成されます。
