---
layout: default
title: ecspresso
nav_order: 1
has_children: true
---

# ecspresso

ecspressoは、AWS Elastic Container Service (ECS) のデプロイと管理を簡素化するためのコマンドラインツールです。インフラストラクチャ・アズ・コードの原則に従って、ECSサービスとタスクを効率的に管理することができます。

## 主な機能

- ECSサービスの作成、更新、削除
- タスク定義の登録、更新
- Blue/Greenデプロイメント（AWS CodeDeployとの統合）
- 一時的なタスクの実行
- 複数環境（開発、ステージング、本番）でのデプロイメント管理
- CI/CDパイプラインとの統合

## ドキュメント構成

このドキュメントは以下のセクションで構成されています：

1. [はじめに](./introduction/about.html) - ecspressoの概要とインストール方法
2. [クイックスタート](./quickstart/basic-usage.html) - 基本的な使い方とよくあるユースケース
3. [コマンドリファレンス](./commands/index.html) - 各コマンドの詳細な説明
4. [実践ガイド](./practical/cicd.html) - CI/CD統合、複数環境運用、大規模サービス管理
5. [トラブルシューティング](./troubleshooting/index.html) - 一般的な問題と解決方法
