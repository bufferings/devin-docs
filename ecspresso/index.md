---
layout: default
title: ecspresso
nav_order: 1
has_children: true
---

# ecspresso

ecspressoは、AWS Elastic Container Service (ECS)のデプロイと管理を簡素化するためのコマンドラインツールです。

## 特徴

- ECSサービスとタスク定義の管理
- ローリングデプロイとBlue/Greenデプロイのサポート
- 複数環境（開発、ステージング、本番）での設定管理
- CI/CDパイプラインとの統合
- AWS CodeDeployとの連携
- Jsonnetによるテンプレート機能
- VPC Lattice、EBS Volumeなどの高度な機能のサポート

## 目次

1. [はじめに](./introduction/about.html)
   - [ecspressoとは](./introduction/about.html)
   - [インストール方法](./introduction/installation.html)
2. [クイックスタート](./quickstart/basic-usage.html)
   - [基本的な使い方](./quickstart/basic-usage.html)
   - [よくあるユースケース](./quickstart/use-cases.html)
3. [コマンドリファレンス](./commands/index.html)
4. [実践ガイド](./practical/index.html)
   - [CI/CDパイプラインとの統合](./practical/cicd.html)
   - [複数環境での運用](./practical/multi-env.html)
   - [大規模サービスの管理](./practical/large-scale.html)
5. [トラブルシューティング](./troubleshooting/index.html)
