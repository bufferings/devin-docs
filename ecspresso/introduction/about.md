---
layout: default
title: ecspressoとは
parent: はじめに
grand_parent: ecspresso
nav_order: 1
---

# ecspressoとは

ecspressoは、AWS Elastic Container Service (ECS)のリソース管理を簡素化するためのコマンドラインツールです。Goで開発されており、ECSサービスのデプロイ、タスク定義の管理、サービスの状態監視など、様々な操作を効率的に行うことができます。

## 主な機能

- ECSサービスのデプロイと管理
- タスク定義の登録と管理
- サービスの状態確認とモニタリング
- AWS CodeDeployを使用したブルー/グリーンデプロイメント
- 一時的なタスクの実行
- 複数環境（開発、ステージング、本番）での設定管理

## アーキテクチャ

ecspressoは、AWS SDK for Goを使用してAWS ECS APIと通信します。設定ファイルとして`ecspresso.yml`を使用し、タスク定義とサービス定義をJSON形式で管理します。

```mermaid
graph TD
    A[ecspresso CLI] --> B[設定ファイル]
    A --> C[タスク定義]
    A --> D[サービス定義]
    A --> E[AWS SDK for Go]
    E --> F[AWS ECS API]
    E --> G[AWS CodeDeploy API]
    E --> H[その他のAWS API]
```

## データフロー

ecspressoを使用したデプロイメントのデータフローは次のようになります：

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Ecspresso as ecspresso
    participant AWS as AWS API
    
    User->>Ecspresso: コマンド実行
    Ecspresso->>Ecspresso: 設定ファイル読み込み
    Ecspresso->>AWS: リソース情報取得
    AWS-->>Ecspresso: 現在の状態を返す
    Ecspresso->>Ecspresso: 差分を計算
    Ecspresso->>User: 差分を表示（必要に応じて）
    User->>Ecspresso: 確認（必要に応じて）
    Ecspresso->>AWS: リソース更新
    AWS-->>Ecspresso: 更新結果
    Ecspresso->>User: 結果表示
```
