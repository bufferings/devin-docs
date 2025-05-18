---
layout: default
title: diff
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 4
---

# diff

`diff`コマンドは、ローカルの設定ファイルと現在のAWS ECSサービスとの差分を表示するためのコマンドです。デプロイ前に変更内容を確認するのに役立ちます。

## 基本的な使い方

```console
$ ecspresso diff [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--envfile` | 環境変数ファイルのパス | - |
| `--ext-str` | Jsonnet用の外部文字列値 | - |
| `--ext-code` | Jsonnet用の外部コード値 | - |
| `--task-definition` | タスク定義の差分のみを表示 | `false` |
| `--service-definition` | サービス定義の差分のみを表示 | `false` |
| `--unified` | 統合差分形式の行数 | `3` |
| `--color` | 差分の色付け（`always`、`never`、`auto`） | `auto` |

## 出力例

### タスク定義の差分

```diff
--- Remote Task Definition
+++ Local Task Definition
@@ -4,7 +4,7 @@
   "containerDefinitions": [
     {
       "name": "web",
-      "image": "nginx:1.21.0",
+      "image": "nginx:1.22.0",
       "essential": true,
       "portMappings": [
         {
@@ -20,7 +20,7 @@
           "awslogs-stream-prefix": "web"
         }
       },
-      "cpu": 256,
+      "cpu": 512,
       "memory": 512,
       "memoryReservation": 256
     }
```

### サービス定義の差分

```diff
--- Remote Service Definition
+++ Local Service Definition
@@ -1,7 +1,7 @@
 {
   "serviceName": "myservice",
-  "desiredCount": 1,
+  "desiredCount": 2,
   "launchType": "FARGATE",
   "networkConfiguration": {
     "awsvpcConfiguration": {
@@ -15,6 +15,12 @@
       "assignPublicIp": "DISABLED"
     }
   },
+  "deploymentConfiguration": {
+    "deploymentCircuitBreaker": {
+      "enable": true,
+      "rollback": true
+    }
+  },
   "loadBalancers": [
     {
       "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890",
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso diff --config ecspresso.yml
```

### 環境変数ファイルを使用

```console
$ ecspresso diff --config ecspresso.yml --envfile production.env
```

### Jsonnet用の外部変数を指定

```console
$ ecspresso diff --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3 --ext-str ENV=production
```

### タスク定義の差分のみを表示

```console
$ ecspresso diff --config ecspresso.yml --task-definition
```

### サービス定義の差分のみを表示

```console
$ ecspresso diff --config ecspresso.yml --service-definition
```

### 差分の表示行数を変更

```console
$ ecspresso diff --config ecspresso.yml --unified 5
```

### 色付けを無効化

```console
$ ecspresso diff --config ecspresso.yml --color never
```

## 差分の解釈

`diff`コマンドの出力は、Unix `diff`コマンドの形式に従います：

- `-` で始まる行は、リモート（AWS ECS）の設定にあるが、ローカルの設定にない行を示します
- `+` で始まる行は、ローカルの設定にあるが、リモート（AWS ECS）の設定にない行を示します
- `@@ -X,Y +P,Q @@` は、差分のある行の範囲を示します（リモートのX行目からY行、ローカルのP行目からQ行）

## 主な差分の種類

### 1. イメージタグの変更

```diff
-      "image": "nginx:1.21.0",
+      "image": "nginx:1.22.0",
```

これは、コンテナイメージのバージョンが変更されたことを示します。

### 2. リソース割り当ての変更

```diff
-      "cpu": 256,
+      "cpu": 512,
```

これは、CPUリソースの割り当てが増加したことを示します。

### 3. タスク数の変更

```diff
-  "desiredCount": 1,
+  "desiredCount": 2,
```

これは、希望するタスク数が1から2に増加したことを示します。

### 4. 設定の追加

```diff
+  "deploymentConfiguration": {
+    "deploymentCircuitBreaker": {
+      "enable": true,
+      "rollback": true
+    }
+  },
```

これは、デプロイメントサーキットブレーカーの設定が追加されたことを示します。

## CI/CDパイプラインでの使用

`diff`コマンドは、CI/CDパイプラインでデプロイ前の変更確認に役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  diff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          ecspresso diff --config ecspresso.yml > diff.txt
      - uses: actions/upload-artifact@v3
        with:
          name: diff
          path: diff.txt
```

## 注意事項

- `diff`コマンドは、AWSリソースとの通信を行うため、AWS認証情報が正しく設定されている必要があります
- テンプレート変数を使用している場合は、`--envfile`または`--ext-str`オプションで値を指定する必要があります
- Jsonnetファイルを使用している場合は、`--ext-str`または`--ext-code`オプションで外部変数を渡すことができます
- `--task-definition`または`--service-definition`オプションを指定しない場合は、両方の差分が表示されます
- `diff`コマンドは、デプロイ前の変更確認に使用することをお勧めします
- CI/CDパイプラインで`diff`コマンドを実行することで、変更内容を記録できます

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [verify](./verify.html) - 設定ファイルの検証
- [render](./render.html) - 設定ファイルをレンダリングして標準出力に表示
- [status](./status.html) - サービスの状態を表示
