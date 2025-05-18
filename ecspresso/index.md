---
layout: default
title: ecspresso
nav_order: 1
has_children: true
---

# ecspresso

ecspressoは、AWS Elastic Container Service (ECS)のデプロイと管理を簡単にするためのツールです。

## 目次

1. [はじめに](./introduction/about.html)
   - [ecspressoについて](./introduction/about.html)
   - [インストール](./introduction/installation.html)

2. [クイックスタート](./quickstart/basic-usage.html)
   - [基本的な使い方](./quickstart/basic-usage.html)
   - [ユースケース](./quickstart/use-cases.html)

3. [コマンドリファレンス](./commands/index.html)
   - [appspec](./commands/appspec.html)
   - [delete](./commands/delete.html)
   - [deploy](./commands/deploy.html)
   - [deregister](./commands/deregister.html)
   - [diff](./commands/diff.html)
   - [exec](./commands/exec.html)
   - [init](./commands/init.html)
   - [refresh](./commands/refresh.html)
   - [register](./commands/register.html)
   - [render](./commands/render.html)
   - [revisions](./commands/revisions.html)
   - [rollback](./commands/rollback.html)
   - [run](./commands/run.html)
   - [scale](./commands/scale.html)
   - [status](./commands/status.html)
   - [tasks](./commands/tasks.html)
   - [verify](./commands/verify.html)
   - [wait](./commands/wait.html)
   - [version](./commands/version.html)

4. [実践ガイド](./practical/cicd.html)
   - [CI/CDパイプライン統合](./practical/cicd.html)
   - [大規模サービス管理](./practical/large-scale.html)
   - [複数環境管理](./practical/multi-env.html)

5. [トラブルシューティング](./troubleshooting/index.html)

## 特徴

- ECSサービスのデプロイ（ローリングデプロイとBlue/Greenデプロイをサポート）
- タスク定義の登録と管理
- 一時的なタスクの実行
- サービスの状態確認
- 設定ファイルの検証と表示
- AWS CodeDeployとの統合
- Jsonnetテンプレートのサポート
- プラグインシステム
