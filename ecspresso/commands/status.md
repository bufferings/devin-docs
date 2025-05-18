---
layout: default
title: status
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 3
---

# status

`status`コマンドは、ECSサービスの現在の状態を表示するために使用します。

## 基本的な使い方

```bash
ecspresso status [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--events N` | 表示するイベント数 | 10 |

## 使用例

### 基本的な使用法

```bash
ecspresso status
```

### より多くのイベントを表示

```bash
ecspresso status --events 20
```

## 出力例

```
Service: myapp
Cluster: application
TaskDefinition: myapp:10
Deployments:
  PRIMARY myapp:10 desired=2 pending=0 running=2
TaskSets:
AutoScaling:
  ScalableTarget - MIN: 1 MAX: 4
  ScalingPolicy - CPU_UTILIZATION - Target: 75.0%
Events:
  2023-05-01 12:34:56 (service myapp) has reached a steady state.
  2023-05-01 12:33:45 (service myapp) has begun draining connections on 1 tasks.
  2023-05-01 12:32:34 (service myapp) registered 1 new tasks.
```

## 詳細

`status`コマンドは以下の情報を表示します：

- サービス名、クラスター名、タスク定義
- デプロイメント状況（デザイアカウント、実行中タスク数など）
- タスクセット情報（CodeDeployを使用している場合）
- オートスケーリング設定（設定されている場合）
- 最近のサービスイベント

この情報は、サービスの現在の状態を確認し、問題を診断するのに役立ちます。
