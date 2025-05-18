---
layout: default
title: status
parent: コマンドリファレンス
nav_order: 4
---

# status

`status`コマンドは、ECSサービスの現在の状態を表示します。デプロイメント、タスクセット、Auto Scaling設定、最近のイベントなどの情報を確認できます。

## 基本的な使い方

```bash
ecspresso status
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--events` | 表示するイベント数 | `10` |

## 表示される情報

`status`コマンドは以下の情報を表示します：

1. **サービス情報**
   - サービス名
   - クラスター名
   - タスク定義
   - 起動タイプ
   - プラットフォームバージョン
   - ヘルスチェック猶予期間
   - デプロイメントコントローラータイプ

2. **デプロイメント情報**
   - デプロイメントID
   - ステータス
   - タスク数（実行中/保留中/希望数）
   - 更新日時

3. **タスクセット情報**（CodeDeployを使用している場合）
   - タスクセットID
   - ステータス
   - タスク数
   - トラフィック％

4. **Auto Scaling情報**
   - スケーリングポリシー
   - 最小容量
   - 最大容量
   - 現在の容量

5. **イベント情報**
   - 最近のサービスイベント（デフォルトで10件）
   - イベントの日時
   - イベントのメッセージ

## 出力例

```
Service: your-service
  Status: ACTIVE
  TaskDefinition: your-task-definition:3
  Deployments:
    PRIMARY(id: ecs-svc/1234567890123456789)
      Status: PRIMARY
      TaskDefinition: your-task-definition:3
      DesiredCount: 2
      PendingCount: 0
      RunningCount: 2
      CreatedAt: 2023-01-01 12:00:00 +0000 UTC
      UpdatedAt: 2023-01-01 12:05:00 +0000 UTC
  Events:
    2023-01-01 12:05:00 +0000 UTC: (service your-service) has reached a steady state.
    2023-01-01 12:03:00 +0000 UTC: (service your-service) has started 2 tasks: (task 1234567890123456789) (task 9876543210987654321).
    2023-01-01 12:01:00 +0000 UTC: (service your-service) registered 2 targets in target-group target-group-name
```

## Auto Scaling情報の表示

サービスにApplication Auto Scalingが設定されている場合、`status`コマンドはAuto Scaling情報も表示します：

```
Service: your-service
  ...
  AutoScaling:
    ScalableDimension: ecs:service:DesiredCount
    MinCapacity: 2
    MaxCapacity: 10
    Status: ACTIVE
    Policies:
      - AlarmName: TargetTracking-service/your-cluster/your-service-AlarmHigh-12345678
        PolicyName: your-service-tracking-policy
        PolicyType: TargetTrackingScaling
        TargetValue: 75.0
```

## イベント数の指定

表示するイベント数を変更するには、`--events`オプションを使用します：

```bash
# 最新の20件のイベントを表示
ecspresso status --events=20

# イベントを表示しない
ecspresso status --events=0
```

## 使用例

### 基本的な使用方法

```bash
ecspresso status
```

### より多くのイベントを表示

```bash
ecspresso status --events=30
```

### イベントを表示せずにステータスを確認

```bash
ecspresso status --events=0
```

## 注意事項

- サービスが存在しない場合は、エラーが発生します。
- CodeDeployを使用している場合は、タスクセット情報も表示されます。
- Auto Scalingが設定されていない場合、Auto Scaling情報は表示されません。
