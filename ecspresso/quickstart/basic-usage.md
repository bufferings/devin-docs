---
layout: default
title: 基本的な使い方
parent: クイックスタート
nav_order: 1
---

# 基本的な使い方

ecspressoを使用すると、既存のECSサービスを簡単にコードで管理できます。

## 初期設定

`ecspresso init`コマンドを使用して、既存のECSサービスから設定を生成します。

```console
$ ecspresso init --region ap-northeast-1 --cluster default --service myservice --config ecspresso.yml
2019/10/12 01:31:48 myservice/default save service definition to ecs-service-def.json
2019/10/12 01:31:48 myservice/default save task definition to ecs-task-def.json
2019/10/12 01:31:48 myservice/default save config to ecspresso.yml
```

生成されたファイル（`ecspresso.yml`、`ecs-service-def.json`、`ecs-task-def.json`）を確認します。

## 設定ファイル

`ecspresso.yml`は以下のような構成になっています：

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

## デプロイ

```console
$ ecspresso deploy --config ecspresso.yml
```

## テンプレートの使用

ecspressoはサービス定義とタスク定義ファイルをテンプレートとして読み込むことができます。典型的な使用例はタスク定義ファイルで画像のタグを置き換えることです。

ecs-task-def.jsonを以下のように変更します。

```diff
-  "image": "nginx:latest",
+  "image": "nginx:{% raw %}{{ must_env `IMAGE_TAG` }}{% endraw %}",
```

そして、環境変数`IMAGE_TAG`を指定してサービスをデプロイします。

```console
$ IMAGE_TAG=stable ecspresso deploy --config ecspresso.yml
```

## 差分の確認

デプロイ前に、現在のECSサービスとローカルの定義ファイルの差分を確認できます。

```console
$ ecspresso diff --config ecspresso.yml
```

## 設定の検証

デプロイ前に、設定が正しいかどうかを検証できます。

```console
$ ecspresso verify --config ecspresso.yml
```
