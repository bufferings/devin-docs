---
layout: default
title: よくあるユースケース
parent: クイックスタート
grand_parent: ecspresso
nav_order: 2
---

# よくあるユースケース

## 環境変数を使用した設定のカスタマイズ

環境変数を使用して、異なる環境用に設定を切り替えることができます：

```bash
$ MYSQL_HOST=prod.example.com ecspresso deploy
```

または、環境ファイルを使用する：

```bash
$ ecspresso deploy --envfile=prod.env
```

## Blue/Greenデプロイメント

AWS CodeDeployを使用したBlue/Greenデプロイを実行するには：

```bash
$ ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
```

## サービスのスケーリング

サービスのタスク数を変更するには：

```bash
$ ecspresso deploy --tasks=10
```

または、Auto Scalingを設定する：

```bash
$ ecspresso deploy --suspend-auto-scaling=false --auto-scaling-min=1 --auto-scaling-max=20
```

## デプロイを待たずに次の操作に進む

デプロイの完了を待たずに次の操作に進むには：

```bash
$ ecspresso deploy --no-wait
```

## タスク定義の更新なしでサービスを更新

タスク定義を更新せずにサービス定義のみを更新するには：

```bash
$ ecspresso deploy --skip-task-definition
```

## 強制的に新しいデプロイメントを開始

現在のタスク定義でも強制的に新しいデプロイメントを開始するには：

```bash
$ ecspresso deploy --force-new-deployment
```
