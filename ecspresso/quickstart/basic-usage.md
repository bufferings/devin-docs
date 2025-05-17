---
layout: default
title: 基本的な使い方
parent: クイックスタート
grand_parent: ecspresso
nav_order: 1
---

# 基本的な使い方

ecspressoを使い始めるための基本的な手順を説明します。

## 設定ファイルの初期化

既存のECSサービスから設定ファイルを作成するには、`init`コマンドを使用します：

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice
```

このコマンドは以下のファイルを生成します：
- `ecspresso.yml`: ecspressoの設定ファイル
- `ecs-service-def.json`: ECSサービス定義
- `ecs-task-def.json`: ECSタスク定義

## サービスのデプロイ

サービスをデプロイするには、`deploy`コマンドを使用します：

```console
$ ecspresso deploy
```

このコマンドは、タスク定義を登録し、サービスを更新します。

## サービスの状態確認

サービスの状態を確認するには、`status`コマンドを使用します：

```console
$ ecspresso status
```

## タスクの実行

一時的なタスクを実行するには、`run`コマンドを使用します：

```console
$ ecspresso run
```

## 設定の差分確認

現在のサービスと設定ファイルの差分を確認するには、`diff`コマンドを使用します：

```console
$ ecspresso diff
```

## 基本的なワークフロー

```mermaid
graph LR
  A[init] --> B[設定ファイル編集]
  B --> C[diff]
  C --> D[deploy]
  D --> E[status]
```

1. `init`で設定ファイルを作成
2. 必要に応じて設定ファイルを編集
3. `diff`で変更内容を確認
4. `deploy`でサービスを更新
5. `status`でサービスの状態を確認
