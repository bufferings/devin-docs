---
layout: default
title: CI/CDパイプラインとの統合
parent: 実践ガイド
grand_parent: ecspresso
nav_order: 1
---

# CI/CDパイプラインとの統合

ecspressoは、様々なCI/CDパイプラインと簡単に統合できます。このページでは、一般的なCI/CDツールとの統合方法を紹介します。

## GitHub Actions

GitHub Actionsでecspressoを使用する例：

```yaml
name: Deploy to ECS

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Install ecspresso
        uses: kayac/ecspresso-action@v1
        
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1
          
      - name: Deploy to ECS
        run: ecspresso deploy --config ecspresso.yml
```

## CircleCI

CircleCIでecspressoを使用する例：

```yaml
version: 2.1

orbs:
  ecspresso: fujiwara/ecspresso@1.0

jobs:
  deploy:
    docker:
      - image: cimg/base:2023.03
    steps:
      - checkout
      - ecspresso/install
      - aws-cli/setup:
          aws-access-key-id: AWS_ACCESS_KEY_ID
          aws-secret-access-key: AWS_SECRET_ACCESS_KEY
          aws-region: AWS_REGION
      - run:
          name: Deploy to ECS
          command: ecspresso deploy --config ecspresso.yml

workflows:
  version: 2
  deploy:
    jobs:
      - deploy:
          filters:
            branches:
              only: main
```

## AWS CodeBuild

AWS CodeBuildでecspressoを使用する例：

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      golang: 1.20
    commands:
      - curl -sL https://github.com/kayac/ecspresso/releases/download/latest/ecspresso_linux_amd64.tar.gz | tar xzC /tmp
      - sudo install /tmp/ecspresso /usr/local/bin/ecspresso
      
  build:
    commands:
      - ecspresso deploy --config ecspresso.yml

artifacts:
  files:
    - appspec.yml
    - taskdef.json
```

## デプロイパイプラインの設計

ecspressoを使用した一般的なデプロイパイプラインは以下のようになります：

```mermaid
graph LR
    A[コード変更] --> B[ビルド]
    B --> C[テスト]
    C --> D[コンテナイメージ作成]
    D --> E[イメージプッシュ]
    E --> F[タスク定義更新]
    F --> G[ecspresso deploy]
    G --> H[デプロイ監視]
    H --> I[検証]
```

## 環境別の設定管理

複数環境（開発、ステージング、本番）でのデプロイを管理するには、環境変数ファイルを使用します：

```bash
# 開発環境へのデプロイ
ecspresso deploy --config ecspresso.yml --envfile dev.env

# ステージング環境へのデプロイ
ecspresso deploy --config ecspresso.yml --envfile staging.env

# 本番環境へのデプロイ
ecspresso deploy --config ecspresso.yml --envfile production.env
```

環境変数ファイルの例：

```
# dev.env
DESIRED_COUNT=1
MEMORY=512
CPU=256

# production.env
DESIRED_COUNT=3
MEMORY=1024
CPU=512
```

## セキュリティのベストプラクティス

CI/CDパイプラインでecspressoを使用する際のセキュリティのベストプラクティス：

1. 最小権限の原則に従ったIAMポリシーを使用する
2. 機密情報はシークレット管理サービスを使用して保存する
3. デプロイ用のIAMロールを作成し、必要な権限のみを付与する
4. 本番環境へのデプロイには承認ステップを追加する
