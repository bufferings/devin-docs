---
layout: default
title: diff
parent: コマンドリファレンス
nav_order: 5
---

# diff

`diff`コマンドは、ローカルのタスク定義・サービス定義と、現在実行中のサービスとタスク定義の差分を表示します。

## 使い方

```console
$ ecspresso diff --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--service-definition` | サービス定義のJSONファイルパス |
| `--compare-task-definition` | タスク定義の差分を表示（デフォルト: true） |
| `--no-compare-task-definition` | タスク定義の差分を表示しない |
| `--compare-service` | サービス定義の差分を表示（デフォルト: true） |
| `--no-compare-service` | サービス定義の差分を表示しない |
| `--unified` | 統合差分形式の行数（デフォルト: 3） |

## 使用例

### 基本的な使用方法

```console
$ ecspresso diff --config ecspresso.yml
```

### タスク定義のみの差分を表示

```console
$ ecspresso diff --config ecspresso.yml --no-compare-service
```

### サービス定義のみの差分を表示

```console
$ ecspresso diff --config ecspresso.yml --no-compare-task-definition
```

### 統合差分形式の行数を変更

```console
$ ecspresso diff --config ecspresso.yml --unified 5
```

## 出力例

```diff
--- ecs-service-def.json
+++ ecs-service-def.json
@@ -1,7 +1,7 @@
 {
   "desiredCount": 1,
   "enableECSManagedTags": false,
-  "launchType": "FARGATE",
+  "launchType": "EC2",
   "loadBalancers": [
     {
       "containerName": "nginx",
```

## 注意事項

- このコマンドは実際にサービスやタスク定義を変更しません。
- デプロイ前に変更内容を確認するために使用します。
- 差分がない場合は何も出力されません。
