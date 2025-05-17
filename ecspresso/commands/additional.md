---
layout: default
title: その他のコマンド
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 5
---

# その他のコマンド

## register

タスク定義を登録します。

```
Usage: ecspresso register [flags]

Flags:
  --dry-run             ドライラン（実際の変更は行わない）
  --output=FORMAT       出力フォーマット（json, yaml, jsonnet）（デフォルト：json）
```

### 例

タスク定義の登録：
```console
$ ecspresso register
```

## deregister

タスク定義の登録を解除します。

```
Usage: ecspresso deregister [flags]

Flags:
  --dry-run             ドライラン（実際の変更は行わない）
  --revision=N          登録解除するリビジョン番号（デフォルト：現在のリビジョン）
```

### 例

現在のタスク定義の登録解除：
```console
$ ecspresso deregister
```

特定リビジョンの登録解除：
```console
$ ecspresso deregister --revision=10
```

## delete

サービスを削除します。

```
Usage: ecspresso delete [flags]

Flags:
  --dry-run             ドライラン（実際の変更は行わない）
  --force               確認なしで削除
```

### 例

サービスの削除：
```console
$ ecspresso delete
```

## appspec

CodeDeployのAppSpec YAMLを標準出力に出力します。

```
Usage: ecspresso appspec [flags]

Flags:
  --task-definition=ARN  使用するタスク定義ARN（デフォルト：現在のタスク定義）
  --output=FILE          出力ファイル（デフォルト：標準出力）
```

### 例

AppSpecの生成：
```console
$ ecspresso appspec > appspec.yaml
```

特定のタスク定義を使用したAppSpec生成：
```console
$ ecspresso appspec --task-definition=arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myapp:10
```
