---
layout: default
title: ecspresso
nav_order: 1
has_children: true
permalink: /ecspresso
---

# ecspresso ドキュメント

Amazon ECS向けのデプロイツール「ecspresso」の公式ドキュメントへようこそ。

ecspressoは、Amazon ECSサービスとタスクを簡単に管理するためのツールです。コード化されたタスク定義とサービス定義を使用して、ECSリソースをデプロイ、更新、監視することができます。

## 主な機能

- **シンプルなデプロイ**: 既存のECSサービスを簡単に更新
- **テンプレート機能**: 環境変数を使用して設定をカスタマイズ
- **Blue/Greenデプロイ**: AWS CodeDeployを使用した無停止デプロイ
- **タスク管理**: ワンタイムタスクの実行とログ監視
- **リソース検証**: サービス設定の検証とトラブルシューティング
- **Jsonnetサポート**: 高度な設定管理

## クイックスタート

```console
# インストール（macOSの場合）
$ brew install kayac/tap/ecspresso

# 既存のサービスから設定を初期化
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice

# サービスをデプロイ
$ ecspresso deploy
```

詳細については、各セクションを参照してください。
