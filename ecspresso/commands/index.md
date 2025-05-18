---
layout: default
title: コマンドリファレンス
nav_order: 3
has_children: true
parent: ecspresso
---

# コマンドリファレンス

ecspressoで利用可能なすべてのコマンドの詳細なリファレンスです。各コマンドの使用方法、オプション、および実例を説明します。

## 共通オプション

すべてのecspressoコマンドで利用可能な共通オプションがあります：

| オプション | 環境変数 | 説明 |
|------------|----------|------|
| `--config` | `ECSPRESSO_CONFIG` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--envfile` | `ECSPRESSO_ENVFILE` | 環境変数ファイル |
| `--debug` | `ECSPRESSO_DEBUG` | デバッグログを有効化 |
| `--ext-str KEY=VALUE` | `ECSPRESSO_EXT_STR` | Jsonnet用の外部文字列値 |
| `--ext-code KEY=VALUE` | `ECSPRESSO_EXT_CODE` | Jsonnet用の外部コード値 |
| `--assume-role-arn` | `ECSPRESSO_ASSUME_ROLE_ARN` | 引き受けるロールのARN |
| `--timeout` | `ECSPRESSO_TIMEOUT` | タイムアウト（設定ファイルの値を上書き） |
| `--filter-command` | `ECSPRESSO_FILTER_COMMAND` | フィルターコマンド |
| `--color/--no-color` | `ECSPRESSO_COLOR` | カラー出力の有効化/無効化 |
| `--log-format` | `ECSPRESSO_LOG_FORMAT` | ログ形式（text, json） |

## コマンド一覧

ecspressoには以下のコマンドが用意されています：

| コマンド | 説明 |
|----------|------|
| [appspec](./appspec.html) | CodeDeployのAppSpec YAMLをSTDOUTに出力 |
| [delete](./delete.html) | サービスを削除 |
| [deploy](./deploy.html) | サービスをデプロイ |
| [deregister](./deregister.html) | タスク定義を登録解除 |
| [diff](./diff.html) | タスク定義、サービス定義と実行中のサービス間の差分を表示 |
| [exec](./exec.html) | タスク上でコマンドを実行 |
| [init](./init.html) | 既存のECSサービスから設定ファイルを作成 |
| [refresh](./refresh.html) | サービスを更新（deploy --skip-task-definition --force-new-deployment --no-update-serviceと同等） |
| [register](./register.html) | タスク定義を登録 |
| [render](./render.html) | 設定、サービス定義、タスク定義ファイルをSTDOUTにレンダリング |
| [revisions](./revisions.html) | タスク定義のリビジョンを表示 |
| [rollback](./rollback.html) | サービスをロールバック |
| [run](./run.html) | タスクを実行 |
| [scale](./scale.html) | サービスをスケール（deploy --skip-task-definition --no-update-serviceと同等） |
| [status](./status.html) | サービスの状態を表示 |
| [tasks](./tasks.html) | サービス内またはタスク定義ファミリー内のタスクを一覧表示 |
| [verify](./verify.html) | 設定内のリソースを検証 |
| [wait](./wait.html) | サービスが安定するまで待機 |
| [version](./version.html) | バージョンを表示 |
