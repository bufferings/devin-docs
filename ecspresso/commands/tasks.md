---
layout: default
title: tasks
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 9
---

# tasks

`tasks`コマンドは、ECSサービスに関連するタスクの一覧を表示するためのコマンドです。実行中のタスクや最近停止したタスクの情報を確認できます。

## 基本的な使い方

```console
$ ecspresso tasks [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--status` | 表示するタスクのステータス（`RUNNING`、`STOPPED`、`ALL`） | `RUNNING` |
| `--id` | 特定のタスクIDを表示 | - |
| `--output` | 出力形式（`table`、`json`、`tsv`） | `table` |
| `--find` | タスクIDの一部を指定して検索 | - |
| `--container-instance` | コンテナインスタンスの詳細を表示 | `false` |
| `--show-eni` | Elastic Network Interfaceの詳細を表示 | `false` |
| `--show-stopped-reason` | 停止したタスクの理由を表示 | `false` |

## 出力例

### テーブル形式（デフォルト）

```
ID                                          STATUS    TASK DEFINITION                 STARTED     STOPPED
12345678-1234-1234-1234-123456789012       RUNNING   myservice:3                     1h ago      -
87654321-4321-4321-4321-210987654321       RUNNING   myservice:3                     30m ago     -
```

### JSON形式

```json
[
  {
    "taskArn": "arn:aws:ecs:ap-northeast-1:123456789012:task/default/12345678-1234-1234-1234-123456789012",
    "clusterArn": "arn:aws:ecs:ap-northeast-1:123456789012:cluster/default",
    "taskDefinitionArn": "arn:aws:ecs:ap-northeast-1:123456789012:task-definition/myservice:3",
    "containerInstanceArn": null,
    "overrides": {
      "containerOverrides": []
    },
    "lastStatus": "RUNNING",
    "desiredStatus": "RUNNING",
    "cpu": "256",
    "memory": "512",
    "containers": [
      {
        "containerArn": "arn:aws:ecs:ap-northeast-1:123456789012:container/12345678-1234-1234-1234-123456789012",
        "taskArn": "arn:aws:ecs:ap-northeast-1:123456789012:task/default/12345678-1234-1234-1234-123456789012",
        "name": "web",
        "lastStatus": "RUNNING",
        "networkBindings": [],
        "networkInterfaces": [
          {
            "attachmentId": "12345678-1234-1234-1234-123456789012",
            "privateIpv4Address": "10.0.1.100"
          }
        ],
        "healthStatus": "HEALTHY"
      }
    ],
    "startedAt": "2023-01-01T12:00:00Z",
    "group": "service:myservice",
    "launchType": "FARGATE",
    "attachments": [
      {
        "id": "12345678-1234-1234-1234-123456789012",
        "type": "ElasticNetworkInterface",
        "status": "ATTACHED",
        "details": [
          {
            "name": "subnetId",
            "value": "subnet-12345678"
          },
          {
            "name": "networkInterfaceId",
            "value": "eni-12345678"
          },
          {
            "name": "privateIPv4Address",
            "value": "10.0.1.100"
          }
        ]
      }
    ],
    "healthStatus": "HEALTHY"
  }
]
```

### TSV形式

```
12345678-1234-1234-1234-123456789012	RUNNING	myservice:3	2023-01-01T12:00:00Z	
87654321-4321-4321-4321-210987654321	RUNNING	myservice:3	2023-01-01T12:30:00Z	
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso tasks --config ecspresso.yml
```

### 停止したタスクを表示

```console
$ ecspresso tasks --config ecspresso.yml --status STOPPED
```

### すべてのタスクを表示

```console
$ ecspresso tasks --config ecspresso.yml --status ALL
```

### 特定のタスクIDを表示

```console
$ ecspresso tasks --config ecspresso.yml --id 12345678-1234-1234-1234-123456789012
```

### JSON形式で出力

```console
$ ecspresso tasks --config ecspresso.yml --output json
```

### TSV形式で出力

```console
$ ecspresso tasks --config ecspresso.yml --output tsv
```

### タスクIDの一部を指定して検索

```console
$ ecspresso tasks --config ecspresso.yml --find 1234
```

### コンテナインスタンスの詳細を表示

```console
$ ecspresso tasks --config ecspresso.yml --container-instance
```

### Elastic Network Interfaceの詳細を表示

```console
$ ecspresso tasks --config ecspresso.yml --show-eni
```

### 停止したタスクの理由を表示

```console
$ ecspresso tasks --config ecspresso.yml --status STOPPED --show-stopped-reason
```

## 表示される情報

`tasks`コマンドは、以下の情報を表示します：

### 基本情報

- **ID**: タスクID
- **STATUS**: タスクのステータス（`RUNNING`、`STOPPED`など）
- **TASK DEFINITION**: タスク定義名とリビジョン
- **STARTED**: タスクの開始時間
- **STOPPED**: タスクの停止時間（停止している場合）

### コンテナインスタンス情報（`--container-instance`オプション）

- **CONTAINER INSTANCE**: コンテナインスタンスID
- **EC2 INSTANCE**: EC2インスタンスID
- **AVAILABILITY ZONE**: アベイラビリティーゾーン

### Elastic Network Interface情報（`--show-eni`オプション）

- **ENI ID**: Elastic Network InterfaceのID
- **SUBNET ID**: サブネットID
- **PRIVATE IP**: プライベートIPアドレス

### 停止理由（`--show-stopped-reason`オプション）

- **STOPPED REASON**: タスクが停止した理由

## タスクのステータス

タスクのステータスには、以下のような値があります：

- **PROVISIONING**: タスクのリソースをプロビジョニング中
- **PENDING**: タスクの起動準備中
- **ACTIVATING**: タスクのアクティベーション中
- **RUNNING**: タスクが実行中
- **DEACTIVATING**: タスクの非アクティブ化中
- **STOPPING**: タスクの停止中
- **DEPROVISIONING**: タスクのリソースを解放中
- **STOPPED**: タスクが停止

## 停止理由

タスクが停止した理由には、以下のような値があります：

- **Essential container in task exited**: 必須コンテナが終了
- **Task failed container health checks**: コンテナのヘルスチェックに失敗
- **Task failed ELB health checks**: ELBのヘルスチェックに失敗
- **Task was stopped by user**: ユーザーによる停止
- **Service scheduler initiated**: サービススケジューラによる停止
- **Spot instance interruption**: スポットインスタンスの中断

## CI/CDパイプラインでの使用

`tasks`コマンドは、CI/CDパイプラインでデプロイ後のタスク状態を確認するのに役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          ecspresso deploy --config ecspresso.yml
          ecspresso wait --config ecspresso.yml
          ecspresso tasks --config ecspresso.yml --output json > tasks.json
      - uses: actions/upload-artifact@v3
        with:
          name: tasks
          path: tasks.json
```

## 注意事項

- `tasks`コマンドは、AWSリソースとの通信を行うため、AWS認証情報が正しく設定されている必要があります
- `--status ALL`オプションを使用すると、実行中のタスクと停止したタスクの両方が表示されます
- `--output json`オプションを使用すると、より詳細な情報がJSON形式で表示されます
- `--find`オプションは、タスクIDの一部を指定して検索する場合に便利です
- `--container-instance`オプションは、EC2起動タイプのタスクでのみ有効な情報を表示します
- `--show-eni`オプションは、awsvpcネットワークモードのタスクでのみ有効な情報を表示します
- `--show-stopped-reason`オプションは、停止したタスクの理由を表示する場合に便利です

## 関連コマンド

- [status](./status.html) - サービスの状態を表示
- [exec](./exec.html) - タスク内でコマンドを実行
- [run](./run.html) - 一時的なタスクを実行
- [deploy](./deploy.html) - サービスをデプロイ
