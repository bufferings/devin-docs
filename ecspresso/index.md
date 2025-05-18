---
layout: default
title: ecspresso
has_children: true
nav_order: 1
---

# ecspresso ドキュメント

ecspressoは、AWS Elastic Container Service (ECS)のリソース管理を簡素化するためのコマンドラインツールです。このドキュメントでは、ecspressoの使い方と機能について詳しく説明します。

## 目次

1. [はじめに](introduction/)
   - [ecspressoとは](introduction/about.html)
   - [インストール方法](introduction/installation.html)
2. [クイックスタート](quickstart/)
   - [基本的な使い方](quickstart/basic-usage.html)
   - [よくあるユースケース](quickstart/use-cases.html)
3. [コマンドリファレンス](commands/)
4. [実践ガイド](practical/)
   - [CI/CDパイプラインとの統合](practical/cicd.html)
   - [複数環境での運用](practical/multi-env.html)
   - [大規模サービスの管理](practical/large-scale.html)
5. [トラブルシューティング](troubleshooting/)

## 主な機能

- ECSサービスのデプロイと管理
- タスク定義の登録と管理
- サービスの状態確認とモニタリング
- AWS CodeDeployを使用したブルー/グリーンデプロイメント
- 一時的なタスクの実行
- 複数環境（開発、ステージング、本番）での設定管理

## サポートとフィードバック

ecspressoは[GitHub](https://github.com/kayac/ecspresso)でオープンソースとして開発されています。バグ報告や機能リクエストは、GitHubのIssueで受け付けています。
