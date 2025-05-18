---
layout: default
title: VPC Latticeとの統合
parent: 実践ガイド
grand_parent: ecspresso
nav_order: 4
---

# VPC Latticeとの統合

ecspressoは、AWS VPC Latticeとの統合をサポートしています。このガイドでは、ecspressoを使用してVPC Latticeを設定し、ECSサービスと統合する方法について説明します。

## 目次

1. [VPC Latticeとは](#vpc-latticeとは)
2. [前提条件](#前提条件)
3. [VPC Lattice統合の設定](#vpc-lattice統合の設定)
4. [サービス定義の設定](#サービス定義の設定)
5. [デプロイと検証](#デプロイと検証)
6. [トラブルシューティング](#トラブルシューティング)
7. [ベストプラクティス](#ベストプラクティス)

## VPC Latticeとは

AWS VPC Latticeは、サービスネットワーキングソリューションで、異なるVPC間でのサービスの検出、接続、通信を簡素化します。VPC Latticeを使用すると、以下のことが可能になります：

1. サービスの検出と接続の自動化
2. サービス間通信のセキュリティ強化
3. サービスメッシュの構築
4. マイクロサービスアーキテクチャの実装

ecspressoは、ECSサービスとVPC Latticeの統合をサポートしており、サービスネットワークとサービスの作成、管理を簡素化します。

## 前提条件

VPC Latticeとecspressoを使用するには、以下の前提条件が必要です：

1. AWS CLIがインストールされ、適切に設定されていること
2. ecspresso v2.3.0以降がインストールされていること
3. VPC Latticeサービスネットワークが作成されていること
4. ECSクラスターが設定されていること

### VPC Latticeサービスネットワークの作成

VPC Latticeサービスネットワークを作成するには、以下のコマンドを使用します：

```console
$ aws vpc-lattice create-service-network \
  --name my-service-network \
  --auth-type NONE
```

### VPCとサービスネットワークの関連付け

VPCをサービスネットワークに関連付けるには、以下のコマンドを使用します：

```console
$ aws vpc-lattice create-service-network-vpc-association \
  --service-network-identifier my-service-network \
  --vpc-identifier vpc-12345678 \
  --security-group-ids sg-12345678
```

## VPC Lattice統合の設定

ecspressoでVPC Latticeを使用するには、サービス定義ファイルに`serviceConnectConfiguration`と`vpcLatticeConfiguration`を設定する必要があります。

### ecspresso.ymlの設定

```yaml
region: ap-northeast-1
cluster: my-cluster
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

## サービス定義の設定

### サービス定義ファイルの例

```json
{
  "serviceName": "myservice",
  "desiredCount": 3,
  "launchType": "FARGATE",
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
  },
  "serviceConnectConfiguration": {
    "enabled": true,
    "namespace": "my-namespace",
    "services": [
      {
        "portName": "http",
        "clientAliases": [
          {
            "port": 80,
            "dnsName": "myservice"
          }
        ]
      }
    ]
  },
  "vpcLatticeConfiguration": {
    "enabled": true,
    "serviceArn": "arn:aws:vpc-lattice:ap-northeast-1:123456789012:service/svc-12345678901234567",
    "targetGroupArn": "arn:aws:vpc-lattice:ap-northeast-1:123456789012:targetgroup/tg-12345678901234567"
  }
}
```

### タスク定義ファイルの例

```json
{
  "family": "myservice",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:latest",
      "essential": true,
      "portMappings": [
        {
          "name": "http",
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "appProtocol": "http"
        }
      ]
    }
  ],
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "networkMode": "awsvpc",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "cpu": "256",
  "memory": "512"
}
```

## VPC Latticeサービスの作成

VPC Latticeサービスを作成するには、以下のコマンドを使用します：

```console
$ aws vpc-lattice create-service \
  --name myservice \
  --auth-type NONE
```

## VPC Latticeターゲットグループの作成

VPC Latticeターゲットグループを作成するには、以下のコマンドを使用します：

```console
$ aws vpc-lattice create-target-group \
  --name myservice-tg \
  --type INSTANCE \
  --config '{"port":80,"protocol":"HTTP","vpcIdentifier":"vpc-12345678"}'
```

## VPC Latticeリスナーの作成

VPC Latticeリスナーを作成するには、以下のコマンドを使用します：

```console
$ aws vpc-lattice create-listener \
  --service-identifier svc-12345678901234567 \
  --name myservice-listener \
  --protocol HTTP \
  --port 80 \
  --default-action '{"targetGroupIdentifier":"tg-12345678901234567","type":"forward"}'
```

## デプロイと検証

### サービスのデプロイ

```console
$ ecspresso deploy --config ecspresso.yml
```

### サービスのステータス確認

```console
$ ecspresso status --config ecspresso.yml
```

### VPC Lattice接続の検証

VPC Lattice接続を検証するには、以下のコマンドを使用します：

```console
$ aws vpc-lattice list-services
$ aws vpc-lattice list-target-groups
$ aws vpc-lattice list-targets --target-group-identifier tg-12345678901234567
```

## VPC Latticeとサービスディスカバリの違い

VPC LatticeとECS Service Connectは、どちらもサービスディスカバリを提供しますが、以下の違いがあります：

1. **スコープ**: VPC Latticeは複数のVPC間でサービスを接続できますが、ECS Service Connectは単一のVPC内でのみ機能します
2. **機能**: VPC Latticeはより高度なルーティング、認証、認可機能を提供します
3. **統合**: VPC LatticeはECS以外のサービスとも統合できますが、ECS Service ConnectはECSサービス間の接続に特化しています

## VPC Latticeの利点

VPC Latticeを使用する主な利点は以下の通りです：

1. **サービスディスカバリの簡素化**: DNSベースのサービスディスカバリにより、サービス間の接続が簡単になります
2. **セキュリティの強化**: きめ細かいアクセス制御とTLS暗号化により、サービス間通信のセキュリティが向上します
3. **モニタリングと可視性**: サービス間通信の詳細なメトリクスとログが提供されます
4. **スケーラビリティ**: サービスの数が増えても、管理のオーバーヘッドが増加しません

## トラブルシューティング

### エラー: VPC Latticeサービスが見つかりません

```
Error: ResourceNotFoundException: The specified VPC Lattice service was not found.
```

**解決策**:
- VPC Latticeサービスが正しく作成されているか確認してください
- サービスARNが正しいか確認してください
- リージョンが正しいか確認してください

### エラー: VPC Latticeターゲットグループが見つかりません

```
Error: ResourceNotFoundException: The specified VPC Lattice target group was not found.
```

**解決策**:
- VPC Latticeターゲットグループが正しく作成されているか確認してください
- ターゲットグループARNが正しいか確認してください
- リージョンが正しいか確認してください

### エラー: VPC Latticeの権限がありません

```
Error: AccessDeniedException: User is not authorized to perform vpc-lattice:CreateService
```

**解決策**:
- IAMユーザーまたはロールに必要な権限が付与されているか確認してください
- 必要な権限のリストについては、[AWS VPC Latticeのドキュメント](https://docs.aws.amazon.com/vpc-lattice/latest/ug/security_iam_service-with-iam.html)を参照してください

## ベストプラクティス

### 1. 命名規則の統一

VPC Latticeリソースには、一貫した命名規則を使用することをお勧めします。

```
<環境>-<サービス名>-<リソースタイプ>
```

例：
- `prod-api-service`
- `prod-api-targetgroup`
- `prod-api-listener`

### 2. IAMポリシーの最小権限

VPC LatticeリソースにアクセスするためのIAMポリシーには、最小限の権限を付与することをお勧めします。

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "vpc-lattice:CreateService",
        "vpc-lattice:DeleteService",
        "vpc-lattice:CreateTargetGroup",
        "vpc-lattice:DeleteTargetGroup",
        "vpc-lattice:RegisterTargets",
        "vpc-lattice:DeregisterTargets"
      ],
      "Resource": "*"
    }
  ]
}
```

### 3. ヘルスチェックの設定

VPC Latticeターゲットグループには、適切なヘルスチェックを設定することをお勧めします。

```console
$ aws vpc-lattice update-target-group \
  --target-group-identifier tg-12345678901234567 \
  --health-check '{"enabled":true,"protocol":"HTTP","path":"/health","port":80,"healthCheckIntervalSeconds":30,"healthCheckTimeoutSeconds":5,"healthyThresholdCount":5,"unhealthyThresholdCount":2}'
```

### 4. アクセスログの有効化

VPC Latticeサービスのアクセスログを有効にすることをお勧めします。

```console
$ aws vpc-lattice update-service \
  --service-identifier svc-12345678901234567 \
  --access-log-configuration '{"enabled":true,"destinationType":"S3","destination":"arn:aws:s3:::my-access-logs"}'
```

### 5. タグの使用

VPC Latticeリソースには、タグを使用して管理を簡素化することをお勧めします。

```console
$ aws vpc-lattice tag-resource \
  --resource-arn arn:aws:vpc-lattice:ap-northeast-1:123456789012:service/svc-12345678901234567 \
  --tags Key=Environment,Value=Production Key=Service,Value=API
```

## CI/CDパイプラインでの使用

VPC Lattice統合は、CI/CDパイプラインで自動化することができます。以下は、GitHub Actionsでの使用例です：

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
          # VPC Latticeサービスの作成
          aws vpc-lattice create-service \
            --name myservice \
            --auth-type NONE
          
          # VPC Latticeターゲットグループの作成
          aws vpc-lattice create-target-group \
            --name myservice-tg \
            --type INSTANCE \
            --config '{"port":80,"protocol":"HTTP","vpcIdentifier":"vpc-12345678"}'
          
          # VPC Latticeリスナーの作成
          aws vpc-lattice create-listener \
            --service-identifier svc-12345678901234567 \
            --name myservice-listener \
            --protocol HTTP \
            --port 80 \
            --default-action '{"targetGroupIdentifier":"tg-12345678901234567","type":"forward"}'
          
          # サービス定義ファイルの更新
          jq '.vpcLatticeConfiguration = {"enabled":true,"serviceArn":"arn:aws:vpc-lattice:ap-northeast-1:123456789012:service/svc-12345678901234567","targetGroupArn":"arn:aws:vpc-lattice:ap-northeast-1:123456789012:targetgroup/tg-12345678901234567"}' ecs-service-def.json > ecs-service-def.json.new
          mv ecs-service-def.json.new ecs-service-def.json
          
          # デプロイ
          ecspresso deploy --config ecspresso.yml
```

## 関連コマンド

- [deploy](../commands/deploy.html) - サービスをデプロイ
- [status](../commands/status.html) - サービスの状態を表示
- [wait](../commands/wait.html) - サービスが安定状態になるまで待機
