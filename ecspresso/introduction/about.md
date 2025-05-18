---
layout: default
title: ecspressoについて
nav_order: 1
parent: はじめに
grand_parent: ecspresso
---

# ecspressoについて

ecspressoは、AWS Elastic Container Service (ECS)のデプロイと管理を簡単にするためのツールです。AWSのCLIやコンソールよりも簡潔なコマンドで、ECSのサービスやタスク定義を扱うことができます。

## 主な機能

- ECSサービスのデプロイ（ローリングデプロイとBlue/Greenデプロイをサポート）
- タスク定義の登録と管理
- 一時的なタスクの実行
- サービスの状態確認
- 設定ファイルの検証と表示

## アーキテクチャ概要

```mermaid
graph TD
    A[ecspresso] --> B[AWS ECS]
    A --> C[AWS CodeDeploy]
    A --> D[AWS CloudFormation]
    A --> E[AWS SSM Parameter Store]
    A --> F[AWS Secrets Manager]
    B --> G[ECSサービス]
    B --> H[ECSタスク定義]
    B --> I[ECSタスク]
```

ecspressoはAWS SDKを使用して、ECSおよび関連するAWSサービスと通信します。設定ファイル（ecspresso.yml）と定義ファイル（ecs-task-def.json、ecs-service-def.json）を使用して、ECSリソースを管理します。
