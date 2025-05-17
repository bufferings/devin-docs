---
layout: default
title: よくあるユースケース
parent: クイックスタート
grand_parent: ecspresso
nav_order: 2
---

# よくあるユースケース

ecspressoの一般的なユースケースを紹介します。

## 1. 既存サービスの管理

既に稼働しているECSサービスをecspressoで管理する場合：

```console
# 設定ファイルの初期化
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice

# 設定ファイルをバージョン管理に追加
$ git add ecspresso.yml ecs-service-def.json ecs-task-def.json
$ git commit -m "Add ecspresso configuration"

# 以降はecspressoでデプロイ
$ ecspresso deploy
```

## 2. Blue/Greenデプロイ

AWS CodeDeployを使用したBlue/Greenデプロイを行う場合：

```console
# サービス定義にCodeDeployの設定を追加
# ecs-service-def.jsonを編集し、deploymentControllerを設定

# デプロイ実行
$ ecspresso deploy
```

サービス定義の例：
```json
{
  "deploymentController": {
    "type": "CODE_DEPLOY"
  },
  "loadBalancers": [
    {
      "containerName": "app",
      "containerPort": 80,
      "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/blue/1234567890123456"
    }
  ]
}
```

## 3. タスクの実行とログ監視

一時的なタスクを実行し、ログを監視する場合：

```console
# タスク実行とログ監視
$ ecspresso run --watch-container=app
```

## 4. サービスのスケーリング

サービスのタスク数を変更する場合：

```console
# タスク数を5に変更
$ ecspresso scale --tasks=5
```

## 5. 環境変数を使った複数環境対応

環境ごとに異なる設定を使用する場合：

```console
# 開発環境用の環境変数ファイル
$ cat .env.dev
CLUSTER=dev-cluster
SERVICE=myservice-dev

# 本番環境用の環境変数ファイル
$ cat .env.prod
CLUSTER=prod-cluster
SERVICE=myservice-prod

# 開発環境へのデプロイ
$ ecspresso --envfile=.env.dev deploy

# 本番環境へのデプロイ
$ ecspresso --envfile=.env.prod deploy
```

## 6. CI/CDパイプラインでの利用

GitHub Actionsでの利用例：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: kayac/ecspresso@v2
        with:
          command: deploy
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ap-northeast-1
```
