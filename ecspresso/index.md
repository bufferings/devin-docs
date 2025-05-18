---
layout: default
title: ecspresso
nav_order: 1
has_children: true
---

# ecspresso ドキュメント

ecspressoは、AWS Elastic Container Service (ECS)のデプロイと管理を簡素化するためのコマンドラインツールです。このドキュメントでは、ecspressoの使い方と機能について詳しく説明します。

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
- タスク定義とサービス定義の管理
- ブルー/グリーンデプロイメント（AWS CodeDeploy連携）
- 一時的なタスクの実行
- サービスの状態監視
- 設定ファイルの検証

## サポートとフィードバック

ecspressoは[GitHub](https://github.com/kayac/ecspresso)でオープンソースとして開発されています。バグ報告や機能リクエストは、GitHubのIssueで受け付けています。
