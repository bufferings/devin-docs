---
layout: default
title: verify
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 3
---

# verify

`verify`コマンドは、設定ファイル、タスク定義ファイル、サービス定義ファイルの構文と内容を検証するためのコマンドです。デプロイ前に設定の問題を発見するのに役立ちます。

## 基本的な使い方

```console
$ ecspresso verify [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--envfile` | 環境変数ファイルのパス | - |
| `--ext-str` | Jsonnet用の外部文字列値 | - |
| `--ext-code` | Jsonnet用の外部コード値 | - |

## 検証内容

`verify`コマンドは、以下の項目を検証します：

1. **設定ファイル**
   - 必須フィールドの存在
   - フィールドの型
   - ファイルパスの有効性

2. **タスク定義ファイル**
   - JSON/Jsonnet構文
   - 必須フィールドの存在
   - フィールドの型
   - リソース制限の有効性

3. **サービス定義ファイル**
   - JSON/Jsonnet構文
   - 必須フィールドの存在
   - フィールドの型
   - 設定の整合性

## 出力例

### 検証成功時

```
2023/01/01 12:00:00 [info] myservice/default Starting verify
2023/01/01 12:00:00 [info] myservice/default Loaded config from ecspresso.yml
2023/01/01 12:00:00 [info] myservice/default Loaded task definition from ecs-task-def.json
2023/01/01 12:00:00 [info] myservice/default Loaded service definition from ecs-service-def.json
2023/01/01 12:00:00 [info] myservice/default Verification succeeded!
```

### 検証失敗時

```
2023/01/01 12:00:00 [info] myservice/default Starting verify
2023/01/01 12:00:00 [info] myservice/default Loaded config from ecspresso.yml
2023/01/01 12:00:00 [info] myservice/default Loaded task definition from ecs-task-def.json
2023/01/01 12:00:00 [error] myservice/default Invalid service definition: missing required field "serviceName"
2023/01/01 12:00:00 [error] myservice/default Verification failed!
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso verify --config ecspresso.yml
```

### 環境変数ファイルを使用

```console
$ ecspresso verify --config ecspresso.yml --envfile production.env
```

### Jsonnet用の外部変数を指定

```console
$ ecspresso verify --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3 --ext-str ENV=production
```

## 検証エラーの例と解決方法

### 1. タスク定義の問題

```
[error] Invalid task definition: CPU and Memory values required for Fargate
```

**解決方法**: タスク定義に`cpu`と`memory`フィールドを追加します。

```json
{
  "family": "myservice",
  "cpu": "256",
  "memory": "512",
  // ...
}
```

### 2. サービス定義の問題

```
[error] Invalid service definition: networkConfiguration required for awsvpc network mode
```

**解決方法**: サービス定義に`networkConfiguration`フィールドを追加します。

```json
{
  "networkConfiguration": {
    "awsvpcConfiguration": {
      "subnets": [
        "subnet-12345678",
        "subnet-87654321"
      ],
      "securityGroups": [
        "sg-12345678"
      ],
      "assignPublicIp": "DISABLED"
    }
  }
}
```

### 3. 設定ファイルの問題

```
[error] Invalid config: service_definition file not found
```

**解決方法**: 設定ファイルで指定されたサービス定義ファイルが存在することを確認します。

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json  # このファイルが存在することを確認
task_definition: ecs-task-def.json
```

## CI/CDパイプラインでの使用

`verify`コマンドは、CI/CDパイプラインでデプロイ前の検証に役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          ecspresso verify --config ecspresso.yml
```

## 注意事項

- `verify`コマンドは、ローカルファイルの検証のみを行い、AWSリソースとの整合性は確認しません
- テンプレート変数を使用している場合は、`--envfile`または`--ext-str`オプションで値を指定する必要があります
- Jsonnetファイルを使用している場合は、`--ext-str`または`--ext-code`オプションで外部変数を渡すことができます
- `verify`コマンドは、デプロイ前の問題を発見するために使用することをお勧めします
- CI/CDパイプラインで`verify`コマンドを実行することで、問題のある設定がデプロイされるのを防ぐことができます

## 関連コマンド

- [init](./init.html) - 既存のECSサービスから設定ファイルを生成
- [render](./render.html) - 設定ファイルをレンダリングして標準出力に表示
- [deploy](./deploy.html) - サービスをデプロイ
- [diff](./diff.html) - ローカルの設定ファイルと現在のサービスとの差分を表示
