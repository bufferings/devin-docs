---
layout: default
title: deregister
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 12
---

# deregister

`deregister`コマンドは、ECSタスク定義の登録を解除するためのコマンドです。不要になったタスク定義を削除する場合に使用します。

## 基本的な使い方

```console
$ ecspresso deregister [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--revision` | 登録解除するタスク定義のリビジョン | - |
| `--dry-run` | 実際に登録解除せずに実行内容を表示 | `false` |

## 出力例

```
2023/01/01 12:00:00 [info] myservice/default Starting deregister
2023/01/01 12:00:00 [info] myservice/default Deregistering task definition: arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:3
2023/01/01 12:00:00 [info] myservice/default Task definition is deregistered: arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:3
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso deregister --config ecspresso.yml --revision 3
```

### ドライラン

```console
$ ecspresso deregister --config ecspresso.yml --revision 3 --dry-run
```

## 注意事項

- `deregister`コマンドは、タスク定義の登録を解除するだけで、完全に削除するわけではありません
- 登録解除されたタスク定義は、新しいタスクの起動には使用できなくなりますが、既存のタスクには影響しません
- `--revision`オプションは必須です。登録解除するタスク定義のリビジョンを指定する必要があります
- `--dry-run`オプションを使用すると、実際に登録解除せずに実行内容を確認できます
- タスク定義の登録解除は元に戻せないため、慎重に行ってください
- 現在使用中のタスク定義を登録解除しても、実行中のタスクには影響しませんが、新しいタスクの起動には使用できなくなります

## タスク定義のリビジョン管理

ECSでは、タスク定義のリビジョンが自動的に管理されます。新しいタスク定義を登録するたびに、リビジョン番号が増加します。古いリビジョンは自動的には削除されないため、不要になったリビジョンは`deregister`コマンドで登録解除することをお勧めします。

タスク定義のリビジョンを確認するには、`revisions`コマンドを使用します：

```console
$ ecspresso revisions --config ecspresso.yml
```

## CI/CDパイプラインでの使用

`deregister`コマンドは、CI/CDパイプラインで古いタスク定義を削除するのに役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  cleanup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          # 最新の5つを除くすべてのリビジョンを登録解除
          REVISIONS=$(ecspresso revisions --config ecspresso.yml | sort -n | head -n -5)
          for REV in $REVISIONS; do
            ecspresso deregister --config ecspresso.yml --revision $REV
          done
```

## 関連コマンド

- [register](./register.html) - タスク定義を登録
- [revisions](./revisions.html) - タスク定義のリビジョンを一覧表示
- [deploy](./deploy.html) - サービスをデプロイ
