---
layout: default
title: delete
parent: コマンドリファレンス
nav_order: 2
---

# delete

`delete`コマンドは、ECSサービスを削除します。

## 使い方

```console
$ ecspresso delete --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--force` | 確認プロンプトをスキップして強制的に削除 |

## 使用例

```console
$ ecspresso delete --config ecspresso.yml
```

このコマンドを実行すると、確認プロンプトが表示されます。

```
2019/10/15 22:47:07 myService/default Start deleting service
Service: myService
Cluster: default
Are you sure want to delete? [y/N]
```

`y`を入力すると、サービスが削除されます。

```
2019/10/15 22:47:09 myService/default Deleting service
2019/10/15 22:47:09 myService/default Service is deleted
```

## 注意事項

- サービスを削除すると、関連するタスクも停止されますが、タスク定義は削除されません。
- 削除したサービスは復元できません。
- ロードバランサーやターゲットグループなどの関連リソースは自動的に削除されないため、必要に応じて手動で削除する必要があります。
