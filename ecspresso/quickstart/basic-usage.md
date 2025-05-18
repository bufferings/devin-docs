---
layout: default
title: 基本的な使い方
parent: クイックスタート
grand_parent: ecspresso
nav_order: 1
---

# 基本的な使い方

このページでは、ecspressoの基本的な使い方を紹介します。

## 設定ファイルの初期化

既存のECSサービスからecspressoの設定ファイルを生成するには、`init`コマンドを使用します：

```bash
ecspresso init --region ap-northeast-1 --cluster your-cluster --service your-service
```

これにより、以下のファイルが生成されます：
- `ecspresso.yml`：ecspressoの設定ファイル
- `ecs-service-def.json`：ECSサービス定義
- `ecs-task-def.json`：ECSタスク定義

## 差分の確認

現在のECSサービスとローカルの定義ファイルの差分を確認するには：

```bash
ecspresso diff
```

## サービスのデプロイ

サービスをデプロイするには：

```bash
ecspresso deploy
```

デプロイ前に変更内容を確認するには、`--dry-run`オプションを使用します：

```bash
ecspresso deploy --dry-run
```

## サービスのステータス確認

サービスの現在の状態を確認するには：

```bash
ecspresso status
```

## 設定ファイルの構造

`ecspresso.yml`の基本的な構造は以下の通りです：

```yaml
region: ap-northeast-1
cluster: your-cluster
service: your-service
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

### 主な設定項目

- `region`：AWSリージョン
- `cluster`：ECSクラスター名
- `service`：ECSサービス名
- `service_definition`：サービス定義ファイルのパス
- `task_definition`：タスク定義ファイルのパス
- `timeout`：操作のタイムアウト時間

## デプロイフロー

ecspressoを使用した基本的なデプロイフローは以下のようになります：

```mermaid
graph LR
    A[初期化] -->|init| B[設定ファイル生成]
    B --> C[ローカルで定義を編集]
    C --> D[差分確認]
    D -->|diff| E[変更確認]
    E --> F[デプロイ]
    F -->|deploy| G[サービス更新]
    G --> H[ステータス確認]
    H -->|status| I[完了確認]
```
