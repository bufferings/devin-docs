---
layout: default
title: コマンドリファレンス
nav_order: 3
has_children: true
---

# コマンドリファレンス

このセクションでは、ecspressoの各コマンドの詳細な使い方と利用可能なオプションについて説明します。

## 共通オプション

すべてのecspressoコマンドで使用できる共通オプションは以下の通りです：

| オプション | 説明 | デフォルト値 | 環境変数 |
|------------|------|------------|----------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` | `ECSPRESSO_CONFIG` |
| `--envfile` | 環境変数ファイルのパス | - | `ECSPRESSO_ENVFILE` |
| `--debug` | デバッグログの有効化 | `false` | `ECSPRESSO_DEBUG` |
| `--ext-str` | Jsonnet用の外部文字列値 | - | `ECSPRESSO_EXT_STR` |
| `--ext-code` | Jsonnet用の外部コード値 | - | `ECSPRESSO_EXT_CODE` |
| `--assume-role-arn` | 引き受けるロールのARN | - | `ECSPRESSO_ASSUME_ROLE_ARN` |
| `--timeout` | タイムアウト時間 | 設定ファイルの値 | `ECSPRESSO_TIMEOUT` |
| `--filter-command` | フィルターコマンド | - | `ECSPRESSO_FILTER_COMMAND` |
| `--color` | カラー出力の有効化 | `true` | `ECSPRESSO_COLOR` |
| `--log-format` | ログフォーマット（text/json） | `text` | `ECSPRESSO_LOG_FORMAT` |

## 利用可能なコマンド

ecspressoは以下のコマンドを提供しています：

| コマンド | 説明 |
|----------|------|
| [`init`](init.html) | 既存のECSサービスから設定ファイルを作成 |
| [`deploy`](deploy.html) | サービスをデプロイ |
| [`refresh`](deploy.html#refresh) | サービスを強制的に再デプロイ |
| [`scale`](deploy.html#scale) | サービスのタスク数を変更 |
| [`status`](status.html) | サービスの状態を表示 |
| [`rollback`](rollback.html) | サービスを以前のバージョンにロールバック |
| [`delete`](delete.html) | サービスを削除 |
| [`run`](run.html) | タスクを実行 |
| [`wait`](wait.html) | サービスが安定状態になるまで待機 |
| [`register`](register.html) | タスク定義を登録 |
| [`deregister`](deregister.html) | タスク定義を登録解除 |
| [`revisions`](revisions.html) | タスク定義のリビジョンを表示 |
| [`diff`](diff.html) | 現在のサービスとローカル定義の差分を表示 |
| [`appspec`](appspec.html) | CodeDeploy用のAppSpecを出力 |
| [`verify`](verify.html) | 設定ファイルを検証 |
| [`render`](render.html) | 設定ファイルをレンダリングして出力 |
| [`tasks`](tasks.html) | サービスのタスク一覧を表示 |
| [`exec`](exec.html) | タスク上でコマンドを実行 |
| [`version`](version.html) | バージョン情報を表示 |

各コマンドの詳細については、それぞれのページを参照してください。
