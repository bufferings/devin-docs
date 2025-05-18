---
layout: default
title: verify
parent: コマンドリファレンス
nav_order: 17
---

# verify

`verify`コマンドは、設定内のリソースを検証します。デプロイ前にリソースの問題を発見するのに役立ちます。

## 使い方

```console
$ ecspresso verify --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--service-definition` | サービス定義のJSONファイルパス |
| `--verify-task-definition` | タスク定義を検証（デフォルト: true） |
| `--no-verify-task-definition` | タスク定義を検証しない |
| `--verify-service` | サービスを検証（デフォルト: true） |
| `--no-verify-service` | サービスを検証しない |

## 使用例

### 基本的な使用方法

```console
$ ecspresso verify --config ecspresso.yml
```

### タスク定義のみを検証

```console
$ ecspresso verify --config ecspresso.yml --no-verify-service
```

### サービス定義のみを検証

```console
$ ecspresso verify --config ecspresso.yml --no-verify-task-definition
```

## 検証項目

`verify`コマンドは、以下の項目を検証します：

1. **タスク定義の検証**
   - タスクロールとタスク実行ロールが存在するか
   - コンテナイメージがECRに存在するか
   - シークレットが存在し、読み取り可能か
   - ログ設定が有効か

2. **サービス定義の検証**
   - ECSクラスターが存在するか
   - ターゲットグループが存在するか
   - セキュリティグループが存在するか
   - サブネットが存在するか
   - キャパシティプロバイダーが存在するか

## 出力例

```
2023/04/01 12:34:56 myService/default Verifying resources...
2023/04/01 12:34:56 myService/default Verified task definition
2023/04/01 12:34:56 myService/default Verified service definition
2023/04/01 12:34:56 myService/default All resources are valid!
```

## 使用シナリオ

`verify`コマンドは、以下のような場合に便利です：

1. デプロイ前にリソースの問題を発見したい場合
2. CI/CDパイプラインで設定の検証を行いたい場合
3. 新しい環境に設定をデプロイする前に検証したい場合

## 注意事項

- このコマンドはリソースを変更せず、検証のみを行います。
- 検証に失敗した場合、エラーメッセージが表示されます。
- すべての検証項目をパスしても、デプロイ時に問題が発生する可能性があります。
