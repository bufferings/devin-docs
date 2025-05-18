---
layout: default
title: トラブルシューティング
parent: ecspresso
nav_order: 5
---

# トラブルシューティング

ecspressoを使用する際に発生する可能性のある一般的な問題とその解決方法を紹介します。

## デプロイに関する問題

### サービスが安定しない

**症状**: `ecspresso deploy`コマンドを実行した後、サービスが安定状態に達しない。

**解決策**:
1. `ecspresso status`コマンドでサービスの状態とイベントを確認
2. CloudWatchログでコンテナのエラーを確認
3. タスク定義のリソース設定（CPU、メモリ）が適切か確認
4. ヘルスチェックの設定を確認

```bash
# より多くのイベントを表示して状態を確認
ecspresso status --events 20
```

### CodeDeployデプロイメントの失敗

**症状**: CodeDeployを使用したデプロイメントが失敗する。

**解決策**:
1. CodeDeployコンソールでデプロイメントの詳細を確認
2. AppSpecファイルの設定を確認
3. ロードバランサーのヘルスチェック設定を確認
4. ロールバックオプションを使用してデプロイ

```bash
# 自動ロールバックを有効にしてデプロイ
ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
```

## 設定に関する問題

### タスク定義の登録エラー

**症状**: タスク定義の登録時にエラーが発生する。

**解決策**:
1. タスク定義のJSON構文を確認
2. 必須フィールドが正しく設定されているか確認
3. コンテナ定義のパラメータが有効か確認
4. IAM権限が十分か確認

```bash
# タスク定義の検証
ecspresso verify
```

### サービス定義の更新エラー

**症状**: サービス定義の更新時にエラーが発生する。

**解決策**:
1. サービス定義のJSON構文を確認
2. 変更不可能なパラメータを更新していないか確認
3. ネットワーク設定が正しいか確認
4. IAM権限が十分か確認

```bash
# 差分を確認
ecspresso diff
```

## 権限に関する問題

### 権限不足エラー

**症状**: `AccessDenied`や`UnauthorizedOperation`エラーが発生する。

**解決策**:
1. AWS認証情報が正しく設定されているか確認
2. IAMポリシーに必要な権限が含まれているか確認
3. 必要に応じてIAMポリシーを更新

**必要なIAM権限の例**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecs:DescribeServices",
        "ecs:DescribeTaskDefinition",
        "ecs:RegisterTaskDefinition",
        "ecs:UpdateService",
        "ecs:RunTask",
        "ecs:DescribeTasks",
        "iam:PassRole"
      ],
      "Resource": "*"
    }
  ]
}
```

## その他の問題

### タイムアウトエラー

**症状**: 操作がタイムアウトする。

**解決策**:
1. `--timeout`オプションで長いタイムアウト時間を設定
2. 設定ファイルでタイムアウト設定を変更

```yaml
# ecspresso.yml
timeout: 20m  # 20分のタイムアウト
```

### デバッグ情報の取得

問題のトラブルシューティングには、デバッグモードを有効にすると役立ちます：

```bash
ecspresso --debug deploy
```

### 一般的なエラーメッセージと解決策

| エラーメッセージ | 考えられる原因 | 解決策 |
|----------------|--------------|-------|
| `service xxx is not found` | サービスが存在しない | サービス名とクラスター名を確認 |
| `task definition xxx is not found` | タスク定義が存在しない | タスク定義名とリビジョンを確認 |
| `failed to register task definition` | タスク定義の内容が無効 | タスク定義JSONを確認 |
| `failed to update service` | サービス更新パラメータが無効 | サービス定義JSONを確認 |
| `context deadline exceeded` | 操作がタイムアウト | タイムアウト設定を増やす |
