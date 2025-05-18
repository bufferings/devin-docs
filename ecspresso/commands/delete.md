---
layout: default
title: delete
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 5
---

# delete

`delete`コマンドは、ECSサービスを削除するために使用します。サービスを削除する前に確認が求められます。

## 基本的な使い方

```console
$ ecspresso delete --config ecspresso.yml
```

## オプション

|| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
|| `--dry-run` | 実際に削除せずに、実行される操作を表示します | `false` |
|| `--force` | 確認なしで削除します | `false` |
|| `--terminate` | タスクを終了させて削除します | `false` |

## 使用例

### ドライランモード（実際に削除せずに確認）

```console
$ ecspresso delete --config ecspresso.yml --dry-run
```

### 確認なしで削除

```console
$ ecspresso delete --config ecspresso.yml --force
```

### タスクを終了させて削除

```console
$ ecspresso delete --config ecspresso.yml --terminate
```

## 削除フロー

```mermaid
graph TD
    A[delete開始] --> B[サービス情報取得]
    B --> C{dry-run?}
    C -->|Yes| D[削除操作表示]
    C -->|No| E{force?}
    E -->|Yes| F[確認をスキップ]
    E -->|No| G[サービス名入力を要求]
    G --> H{入力が一致?}
    H -->|Yes| I[削除処理へ進む]
    H -->|No| J[処理中止]
    F --> I
    I --> K{terminate?}
    K -->|Yes| L[タスクを終了させて削除]
    K -->|No| M[タスクを終了させずに削除]
    L --> N[削除完了]
    M --> N
    D --> O[dry-run完了]
    J --> P[削除キャンセル]
```

## 注意事項

- サービスを削除すると、そのサービスに関連するタスクも停止します
- `--force`オプションを使用しない場合、安全のために削除前にサービス名の入力が求められます
- `--terminate`オプションを使用すると、サービスのタスクが強制的に終了します（AWS ECS DeleteServiceのforceパラメータに相当）
- 削除したサービスは復元できないため、慎重に使用してください
