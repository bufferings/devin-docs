---
layout: default
title: よくあるユースケース
parent: クイックスタート
grand_parent: ecspresso
nav_order: 2
---

# よくあるユースケース

## ブルー/グリーンデプロイメント（CodeDeploy使用）

ecspressoはAWS CodeDeployを使用したブルー/グリーンデプロイメントをサポートしています。

### 設定例

```yaml
# ecspresso.yml
region: ap-northeast-1
cluster: your-cluster
service: your-service
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
codedeploy:
  application_name: AppECS-your-cluster-your-service
  deployment_group_name: DgpECS-your-cluster-your-service
```

サービス定義にデプロイメントコントローラーの設定を追加：

```json
{
  "deploymentController": {
    "type": "CODE_DEPLOY"
  }
}
```

### デプロイ実行

```bash
ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
```

`--rollback-events`オプションを使用すると、デプロイメントが失敗した場合に自動的にロールバックします。

## スケーリングの管理

サービスのスケーリングを設定するには：

```bash
# デザイアカウントを変更
ecspresso scale --tasks 5

# オートスケーリングの設定を変更
ecspresso deploy --auto-scaling-min 2 --auto-scaling-max 10
```

## 一時的なタスクの実行

一時的なタスクを実行するには：

```bash
# デフォルトコンテナでコマンドを実行
ecspresso run --command "echo hello"

# 特定のコンテナでコマンドを実行
ecspresso run --command "echo hello" --container app

# 環境変数を設定してタスクを実行
ecspresso run --command "env" --env KEY=VALUE
```

## 複数環境での設定管理

異なる環境（開発、ステージング、本番）での設定管理には、環境変数ファイルを使用できます：

```bash
# 開発環境
ecspresso deploy --envfile dev.env

# ステージング環境
ecspresso deploy --envfile staging.env

# 本番環境
ecspresso deploy --envfile production.env
```

環境変数ファイルの例（`dev.env`）：

```
SERVICE_NAME=myapp-dev
DESIRED_COUNT=1
```

## サービスのロールバック

問題が発生した場合、以前のタスク定義にロールバックできます：

```bash
# 特定のリビジョンにロールバック
ecspresso rollback --revision 3

# 直前のリビジョンにロールバック
ecspresso rollback --deregister
```

## CI/CDパイプラインでの利用例

GitHub Actionsでの利用例：

```yaml
name: Deploy to ECS

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install ecspresso
        uses: kayac/ecspresso-action@v1
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1
      - name: Deploy to ECS
        run: ecspresso deploy --config ecspresso.yml
```
