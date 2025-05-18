---
layout: default
title: delete
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 11
---

# delete

`delete`コマンドは、ECSサービスを削除するためのコマンドです。サービスに関連するリソース（タスク定義は除く）も削除されます。

## 基本的な使い方

```console
$ ecspresso delete [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--force` | 確認プロンプトをスキップ | `false` |
| `--dry-run` | 実際に削除せずに実行内容を表示 | `false` |

## 削除されるリソース

`delete`コマンドは、以下のリソースを削除します：

1. **ECSサービス**
   - サービスに関連付けられたタスクも停止されます

2. **Auto Scaling設定**（存在する場合）
   - スケーラブルターゲット
   - スケーリングポリシー

## 削除されないリソース

以下のリソースは`delete`コマンドでは削除されません：

1. **タスク定義**
   - タスク定義を削除するには、[deregister](./deregister.html)コマンドを使用してください

2. **ロードバランサー**
   - ターゲットグループ
   - リスナー
   - ロードバランサー自体

3. **CloudWatch Logs**
   - ロググループ
   - ログストリーム

## 出力例

### 通常実行

```
2023/01/01 12:00:00 [info] myService/default Starting delete service
2023/01/01 12:00:00 [info] myService/default Deleting service: myService
2023/01/01 12:00:00 [info] myService/default Successfully deleted service: myService
2023/01/01 12:00:00 [info] myService/default Delete service completed!
```

### ドライラン

```
2023/01/01 12:00:00 [info] myService/default Starting delete service
2023/01/01 12:00:00 [info] myService/default DRY RUN: delete service: myService
2023/01/01 12:00:00 [info] myService/default Delete service completed!
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso delete --config ecspresso.yml
```

実行すると、確認プロンプトが表示されます：

```
2023/01/01 12:00:00 [info] myService/default Starting delete service
2023/01/01 12:00:00 [info] myService/default This operation will delete service: myService
2023/01/01 12:00:00 [info] myService/default Are you sure? [y/N]
```

`y`を入力すると、サービスが削除されます。

### 確認プロンプトをスキップ

```console
$ ecspresso delete --config ecspresso.yml --force
```

### ドライランで実行内容を確認

```console
$ ecspresso delete --config ecspresso.yml --dry-run
```

## 注意事項

- サービスを削除すると、実行中のタスクも停止されます
- 削除されたサービスは元に戻せません
- サービスに関連付けられたタスク定義は削除されません
- サービスに関連付けられたロードバランサーやターゲットグループは削除されません
- 重要なサービスを誤って削除しないように、`--dry-run`オプションを使用して実行内容を事前に確認することをお勧めします
- CI/CDパイプラインで使用する場合は、`--force`オプションを使用して確認プロンプトをスキップする必要があります
