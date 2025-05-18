---
layout: default
title: トラブルシューティング
nav_order: 5
---

# トラブルシューティング

ecspressoを使用する際に発生する可能性のある一般的な問題と、その解決方法を説明します。

## よくある問題と解決策

### デプロイに失敗する

**症状**: `ecspresso deploy`コマンドが失敗し、エラーメッセージが表示される。

**考えられる原因と解決策**:

1. **タスク定義の問題**
   - タスク定義のJSONが無効
   - コンテナイメージが存在しない
   - IAMロールが存在しない、または権限が不足している

   **解決策**:
   ```bash
   # タスク定義を検証
   ecspresso verify --task-def
   
   # タスク定義をレンダリングして確認
   ecspresso render --task-def
   ```

2. **サービス定義の問題**
   - サービス定義のJSONが無効
   - ターゲットグループが存在しない
   - セキュリティグループが存在しない

   **解決策**:
   ```bash
   # サービス定義を検証
   ecspresso verify --service-def
   
   # サービス定義をレンダリングして確認
   ecspresso render --service-def
   ```

3. **AWS認証情報の問題**
   - AWS認証情報が設定されていない
   - AWS認証情報の権限が不足している

   **解決策**:
   ```bash
   # AWS認証情報を確認
   aws sts get-caller-identity
   
   # 必要な権限を持つプロファイルを使用
   export AWS_PROFILE=your-profile
   ```

4. **ネットワーク設定の問題**
   - サブネットが存在しない
   - セキュリティグループのルールが適切でない

   **解決策**:
   ```bash
   # 現在のサービスとローカル定義の差分を確認
   ecspresso diff
   ```

### タスクが起動しない

**症状**: デプロイは成功するが、タスクが起動しない、または起動後すぐに停止する。

**考えられる原因と解決策**:

1. **コンテナの問題**
   - コンテナのエントリポイントやコマンドが無効
   - コンテナがクラッシュしている

   **解決策**:
   ```bash
   # タスクを手動で実行して確認
   ecspresso run
   
   # タスクのステータスを確認
   ecspresso status
   ```

2. **リソース不足**
   - CPUやメモリが不足している
   - ディスク容量が不足している

   **解決策**:
   ```bash
   # タスク定義のリソース設定を確認
   ecspresso render --task-def | grep -E "cpu|memory"
   
   # リソース設定を増やして再デプロイ
   # タスク定義を編集後
   ecspresso deploy
   ```

3. **ヘルスチェックの失敗**
   - ヘルスチェックのパスが無効
   - ヘルスチェックのタイムアウトが短すぎる

   **解決策**:
   ```bash
   # サービス定義のヘルスチェック設定を確認
   ecspresso render --service-def | grep -A 10 "healthCheck"
   ```

### ロールバックに失敗する

**症状**: `ecspresso rollback`コマンドが失敗する。

**考えられる原因と解決策**:

1. **以前のタスク定義が存在しない**
   - タスク定義のリビジョンが削除されている

   **解決策**:
   ```bash
   # 利用可能なタスク定義のリビジョンを確認
   ecspresso revisions
   
   # 特定のリビジョンを指定してロールバック
   ecspresso rollback --revision=10
   ```

2. **サービスの状態が不安定**
   - 現在のデプロイが進行中

   **解決策**:
   ```bash
   # サービスの状態を確認
   ecspresso status
   
   # サービスが安定するまで待機
   ecspresso wait
   ```

### 環境変数の展開に問題がある

**症状**: 環境変数が正しく展開されない。

**考えられる原因と解決策**:

1. **環境変数が設定されていない**
   - 環境変数が定義されていない
   - 環境変数ファイルが読み込まれていない

   **解決策**:
   ```bash
   # 環境変数を設定
   export VARIABLE_NAME=value
   
   # 環境変数ファイルを使用
   ecspresso deploy --envfile=your-env-file
   ```

2. **環境変数の構文が正しくない**
   - 環境変数の参照方法が間違っている

   **解決策**:
   ```bash
   # 環境変数の参照を確認
   # 正しい形式: ${VARIABLE_NAME}
   ecspresso render --task-def
   ```

## デバッグ方法

### デバッグログの有効化

詳細なログを表示するには、`--debug`オプションを使用します：

```bash
ecspresso deploy --debug
```

### AWS APIリクエストの確認

AWS SDKのデバッグログを有効にするには、環境変数を設定します：

```bash
export AWS_SDK_GO_DEBUG=true
ecspresso deploy
```

### タスクの実行ログの確認

タスクの実行ログを確認するには、AWS Management Consoleを使用するか、以下のコマンドを実行します：

```bash
# タスクIDを取得
TASK_ID=$(ecspresso tasks --status=RUNNING --id-only | head -n 1)

# タスク上でコマンドを実行
ecspresso exec --task-id=$TASK_ID --command="cat /var/log/app.log"
```

## よくあるエラーメッセージ

### "InvalidParameterException: The container xxx is not valid for the task definition"

**原因**: タスク定義で指定されたコンテナ名が、サービス定義やコマンドラインで指定されたコンテナ名と一致しない。

**解決策**: タスク定義とサービス定義のコンテナ名を一致させます。

### "AccessDeniedException: User is not authorized to perform ecs:UpdateService"

**原因**: AWS認証情報に必要な権限がない。

**解決策**: 適切な権限を持つIAMユーザーまたはロールを使用します。

### "ClientException: The specified revision does not exist"

**原因**: 指定されたタスク定義のリビジョンが存在しない。

**解決策**: 
```bash
# 利用可能なリビジョンを確認
ecspresso revisions

# 存在するリビジョンを指定
ecspresso rollback --revision=<existing-revision>
```

### "InvalidParameterException: The specified target group does not exist"

**原因**: サービス定義で指定されたターゲットグループが存在しない。

**解決策**: 
```bash
# サービス定義を確認
ecspresso render --service-def

# 存在するターゲットグループを指定するようにサービス定義を修正
```

## サポートの受け方

問題が解決しない場合は、以下の方法でサポートを受けることができます：

1. **GitHub Issues**: [kayac/ecspresso](https://github.com/kayac/ecspresso/issues)でイシューを作成
2. **バグレポート**: バグレポートを提出する際は、以下の情報を含めてください：
   - ecspressoのバージョン（`ecspresso version`）
   - 使用しているコマンドとオプション
   - エラーメッセージ
   - 可能であれば、`--debug`オプションを使用した詳細なログ
