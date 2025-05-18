---
layout: default
title: コマンドリファレンス
parent: ecspresso
nav_order: 3
has_children: true
permalink: /ecspresso/commands
---

# コマンドリファレンス

ecspressoは以下のコマンドを提供しています。各コマンドの詳細については、それぞれのページを参照してください。

- [init](init.html) - 既存のECSサービスから設定ファイルを作成
- [deploy](deploy.html) - サービスをデプロイ
- [status](status.html) - サービスのステータスを表示
- [rollback](rollback.html) - サービスをロールバック
- [delete](delete.html) - サービスを削除
- [run](run.html) - タスクを実行
- [wait](wait.html) - サービスが安定するまで待機
- [register](register.html) - タスク定義を登録
- [deregister](deregister.html) - タスク定義を登録解除
- [revisions](revisions.html) - タスク定義のリビジョンを表示
- [diff](diff.html) - タスク定義、サービス定義と現在実行中のサービスとタスク定義の差分を表示
- [appspec](appspec.html) - CodeDeployのためのAppSpec YAMLを出力
- [verify](verify.html) - 設定内のリソースを検証
- [render](render.html) - 設定、サービス定義、タスク定義ファイルを標準出力にレンダリング
- [tasks](tasks.html) - サービス内のタスクまたは同じファミリーを持つタスクを一覧表示
- [exec](exec.html) - タスク上でコマンドを実行
- [refresh](refresh.html) - サービスを更新（deploy --skip-task-definition --force-new-deployment --no-update-serviceと同等）
- [scale](scale.html) - サービスをスケール（deploy --skip-task-definition --no-update-serviceと同等）
- [version](version.html) - バージョンを表示
