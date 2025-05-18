---
layout: default
title: コマンドリファレンス
parent: ecspresso
has_children: true
nav_order: 3
---

# コマンドリファレンス

ecspressoは以下のコマンドを提供しています：

## 設定関連
- [init](init.html) - 設定ファイルを初期化
- [verify](verify.html) - 設定の検証
- [render](render.html) - 設定ファイルのレンダリング

## デプロイ関連
- [deploy](deploy.html) - サービスをデプロイ
- [rollback](rollback.html) - 以前のタスク定義にロールバック
- [refresh](refresh.html) - サービスを再デプロイ
- [scale](scale.html) - サービスをスケーリング
- [register](register.html) - タスク定義を登録
- [deregister](deregister.html) - タスク定義を登録解除
- [delete](delete.html) - サービスを削除
- [appspec](appspec.html) - CodeDeploy用AppSpecを生成

## モニタリング関連
- [status](status.html) - サービスの状態を表示
- [wait](wait.html) - サービスが安定するまで待機
- [exec](exec.html) - 実行中のタスクでコマンドを実行
- [tasks](tasks.html) - タスク一覧を表示
- [revisions](revisions.html) - タスク定義のリビジョン一覧を表示
- [diff](diff.html) - 定義ファイルと実行中サービスの差分を表示

## その他
- [run](run.html) - 一時的なタスクを実行
- [version](version.html) - バージョン情報を表示
