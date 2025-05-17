---
layout: default
title: ecspressoとは
parent: はじめに
grand_parent: ecspresso
nav_order: 1
---

# ecspressoとは

ecspressoは、Amazon ECS（Elastic Container Service）のためのデプロイツールです。コード化されたタスク定義とサービス定義を使用して、ECSリソースをデプロイ、更新、監視することができます。

## 主な機能

- **シンプルなデプロイ**: 既存のECSサービスを簡単に更新
- **テンプレート機能**: 環境変数を使用して設定をカスタマイズ
- **Blue/Greenデプロイ**: AWS CodeDeployを使用した無停止デプロイ
- **タスク管理**: ワンタイムタスクの実行とログ監視
- **リソース検証**: サービス設定の検証とトラブルシューティング
- **Jsonnetサポート**: 高度な設定管理

## アーキテクチャ

```mermaid
graph TD
  A[ecspresso] --> B[AWS ECS]
  A --> C[AWS CodeDeploy]
  A --> D[CloudWatch Logs]
  B --> E[ECSサービス]
  B --> F[ECSタスク]
  C --> G[Blue/Greenデプロイ]
```

## ワークフロー

ecspressoの基本的なワークフローは以下の通りです：

1. **初期化**: 既存のECSサービスから設定ファイルを作成
2. **設定**: タスク定義とサービス定義をカスタマイズ
3. **デプロイ**: 新しいタスク定義を登録し、サービスを更新
4. **監視**: サービスの状態を確認し、ログを監視

```mermaid
sequenceDiagram
  participant User as ユーザー
  participant Ecspresso as ecspresso
  participant ECS as AWS ECS
  
  User->>Ecspresso: init
  Ecspresso->>ECS: サービス情報取得
  ECS-->>Ecspresso: サービス情報
  Ecspresso-->>User: 設定ファイル作成
  
  User->>Ecspresso: deploy
  Ecspresso->>ECS: タスク定義登録
  ECS-->>Ecspresso: タスク定義ARN
  Ecspresso->>ECS: サービス更新
  ECS-->>Ecspresso: 更新状態
  Ecspresso-->>User: デプロイ完了
```
