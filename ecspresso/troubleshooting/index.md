---
layout: default
title: トラブルシューティング
parent: ecspresso
nav_order: 5
---

# トラブルシューティング

ecspressoを使用する際に発生する可能性のある一般的な問題とその解決方法を説明します。

## デプロイの問題

### サービスが安定しない

**症状**: `ecspresso deploy`コマンドを実行した後、サービスが安定状態に達しない。

**解決策**:
1. `ecspresso status`コマンドを実行して、サービスのイベントを確認します。
2. タスク定義のリソース設定（CPU、メモリ）が適切かどうかを確認します。
3. コンテナのヘルスチェック設定を確認します。
4. `ecspresso verify`コマンドを実行して、設定に問題がないか確認します。

### CodeDeployのデプロイに失敗する

**症状**: CodeDeployを使用したBlue/Greenデプロイが失敗する。

**解決策**:
1. CodeDeployのコンソールでデプロイの詳細を確認します。
2. ロードバランサーのターゲットグループ設定を確認します。
3. タスク定義のポートマッピングが正しいことを確認します。
4. `ecspresso appspec`コマンドを実行して、生成されるAppSpecを確認します。

## 設定の問題

### 設定ファイルが見つからない

**症状**: `ecspresso: config file ecspresso.yml not found`というエラーが表示される。

**解決策**:
1. カレントディレクトリに`ecspresso.yml`ファイルが存在することを確認します。
2. 別の設定ファイルを使用する場合は、`--config`オプションを指定します：
   ```bash
   ecspresso deploy --config=custom-config.yml
   ```

### タスク定義の検証エラー

**症状**: タスク定義の検証に失敗する。

**解決策**:
1. `ecspresso verify --task-definition`コマンドを実行して、詳細なエラーメッセージを確認します。
2. タスク定義のJSONフォーマットが正しいことを確認します。
3. 必須フィールドがすべて指定されていることを確認します。
4. コンテナ定義のイメージ名が正しいことを確認します。

## 権限の問題

### AWS認証エラー

**症状**: `AccessDenied`や`UnauthorizedOperation`などのAWS認証エラーが発生する。

**解決策**:
1. AWS認証情報が正しく設定されていることを確認します。
2. 必要なIAMポリシーがアタッチされていることを確認します。
3. 必要に応じて`--assume-role-arn`オプションを使用して、別のロールを引き受けます：
   ```bash
   ecspresso deploy --assume-role-arn=arn:aws:iam::123456789012:role/EcsDeployRole
   ```

### 必要なIAMポリシー

ecspressoを使用するために必要な最小限のIAMポリシー例：

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

**症状**: デプロイ中にタイムアウトエラーが発生する。

**解決策**:
1. `--timeout`オプションを使用して、タイムアウト時間を延長します：
   ```bash
   ecspresso deploy --timeout=30m
   ```
2. または、設定ファイルでタイムアウトを設定します：
   ```yaml
   timeout: 30m
   ```

### デバッグ情報の表示

問題のトラブルシューティングには、デバッグモードを有効にすると役立ちます：

```bash
ecspresso deploy --debug
```

または、環境変数を使用：

```bash
ECSPRESSO_DEBUG=1 ecspresso deploy
```

## よくある質問

### Q: ecspressoはAWS Fargateと互換性がありますか？
A: はい、ecspressoはEC2起動タイプとFargate起動タイプの両方をサポートしています。

### Q: 複数のAWSアカウントにデプロイするにはどうすればよいですか？
A: `--assume-role-arn`オプションを使用して、別のAWSアカウントのロールを引き受けることができます。

### Q: ecspressoはAWS App Meshと統合できますか？
A: はい、タスク定義にApp Mesh設定を含めることで、App Meshと統合できます。
