---
layout: default
title: コマンドリファレンス
parent: ecspresso
has_children: true
nav_order: 3
---

# コマンドリファレンス

ecspressoは、以下のコマンドを提供しています。各コマンドの詳細については、それぞれのページを参照してください。

## 設定コマンド

- [init](./init.html) - 既存のECSサービスから設定ファイルを生成
- [verify](./verify.html) - 設定ファイルの検証
- [render](./render.html) - 設定ファイルをレンダリングして標準出力に表示

## デプロイコマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [rollback](./rollback.html) - 以前のタスク定義にロールバック
- [delete](./delete.html) - サービスを削除
- [register](./register.html) - タスク定義を登録
- [deregister](./deregister.html) - タスク定義を登録解除
- [revisions](./revisions.html) - タスク定義のリビジョンを一覧表示
- [appspec](./appspec.html) - AWS CodeDeployのAppSpecファイルを生成

## モニタリングコマンド

- [status](./status.html) - サービスの状態を表示
- [tasks](./tasks.html) - タスクの一覧を表示
- [wait](./wait.html) - サービスが安定状態になるまで待機
- [exec](./exec.html) - タスク内でコマンドを実行

## その他のコマンド

- [run](./run.html) - 一時的なタスクを実行
- [scale](./scale.html) - サービスのタスク数を変更
- [refresh](./refresh.html) - サービスのタスクを再起動
- [diff](./diff.html) - ローカルの設定ファイルと現在のサービスとの差分を表示
- [version](./version.html) - ecspressoのバージョンを表示

## 共通オプション

すべてのコマンドで使用できる共通オプションは以下の通りです：

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--envfile` | 環境変数ファイルのパス | - |
| `--ext-str` | Jsonnet用の外部文字列値 | - |
| `--ext-code` | Jsonnet用の外部コード値 | - |
| `--debug` | デバッグログを有効化 | `false` |
| `--help` | ヘルプを表示 | `false` |

## 設定ファイル形式

ecspressoは、以下の形式の設定ファイルをサポートしています：

1. **YAML形式** (.yml, .yaml)
2. **JSON形式** (.json)
3. **Jsonnet形式** (.jsonnet)

### 基本的な設定ファイル例

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.json
task_definition: ecs-task-def.json
timeout: 10m
```

### 高度な設定ファイル例

```yaml
region: ap-northeast-1
cluster: default
service: myservice
service_definition: ecs-service-def.jsonnet
task_definition: ecs-task-def.jsonnet
timeout: 10m
plugins:
  - name: tfstate
    config:
      path: terraform.tfstate
      format: tfstate
  - name: ssm
    config:
      path: /path/to/parameters
ignore:
  service:
    - capacityProviderStrategy
    - deploymentConfiguration.deploymentCircuitBreaker
  task_definition:
    - taskDefinitionArn
    - status
    - compatibilities
    - requiresAttributes
    - registeredAt
    - registeredBy
codedeploy:
  application_name: AppECS-default-myservice
  deployment_group_name: DgpECS-default-myservice
  deployment_config_name: CodeDeployDefault.ECSAllAtOnce
  auto_rollback_on_error: true
appspec:
  hooks:
    BeforeInstall:
      - location: scripts/before_install.sh
        timeout: 300
    AfterInstall:
      - location: scripts/after_install.sh
        timeout: 300
```

## Jsonnetサポート

ecspressoは、Jsonnetをサポートしており、より柔軟な設定ファイルを作成できます。Jsonnetファイルを使用するには、拡張子を`.jsonnet`にして、設定ファイルで指定します。

```jsonnet
// ecs-task-def.jsonnet
{
  family: "myservice",
  containerDefinitions: [
    {
      name: "web",
      image: "nginx:" + std.extVar("IMAGE_TAG"),
      essential: true,
      // ...
    }
  ],
  // ...
}
```

外部変数を渡すには、`--ext-str`または`--ext-code`オプションを使用します：

```console
$ ecspresso deploy --config ecspresso.yml --ext-str IMAGE_TAG=v1.2.3
```
