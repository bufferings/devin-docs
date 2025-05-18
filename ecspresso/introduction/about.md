---
layout: default
title: ecspressoとは
parent: はじめに
nav_order: 1
---

# ecspressoとは

ecspressoは、Amazon ECS（Elastic Container Service）向けのデプロイツールです。コマンドラインから簡単にECSサービスの管理ができます。

## 主な機能

- ECSサービスのデプロイと管理
- タスク定義の登録と更新
- Blue/Greenデプロイ（AWS CodeDeployとの連携）
- ECSタスクの実行と管理
- 複数環境での設定管理
- サービスとタスク定義の差分確認
- タスク上でのコマンド実行（exec）

## アーキテクチャ

```mermaid
graph TD
  A[ecspresso CLI] --> B[AWS ECS API]
  A --> C[AWS CodeDeploy API]
  A --> D[テンプレート処理]
  D --> E[タスク定義]
  D --> F[サービス定義]
  E --> B
  F --> B
  A --> G[プラグイン]
  G --> H[tfstate]
  G --> I[外部コマンド]
  G --> J[SSM]
  G --> K[Secrets Manager]
```

## サポートする機能

ecspressoは以下のAWS ECS機能をサポートしています：

- Fargate
- Fargate Spot
- Service Connect
- EBS Volumes
- VPC Lattice

## テンプレート機能

ecspressoはタスク定義とサービス定義のテンプレート処理をサポートしています：

- Go テンプレート構文
- 環境変数の参照
- Jsonnet
