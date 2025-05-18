---
layout: default
title: トラブルシューティング
parent: ecspresso
nav_order: 5
permalink: /ecspresso/troubleshooting
---

# トラブルシューティング

このセクションでは、ecspressoを使用する際に発生する可能性のある一般的な問題と、その解決方法について説明します。

## よくある問題

### デプロイに失敗する

デプロイに失敗する一般的な原因と解決策：

1. **タスク定義の問題**
   - コンテナイメージが存在しない
   - IAMロールが不適切または存在しない
   - リソース制限が不適切

   **解決策**: `ecspresso verify` コマンドを使用して、タスク定義の問題を事前に検出します。

2. **サービス定義の問題**
   - ターゲットグループが存在しない
   - セキュリティグループが適切に設定されていない
   - サブネットが適切に設定されていない

   **解決策**: `ecspresso diff` コマンドを使用して、変更内容を事前に確認します。

3. **AWS APIエラー**
   - APIリクエスト制限に達した
   - 認証情報の問題

   **解決策**: AWS認証情報が正しく設定されていることを確認し、必要に応じてリトライします。

### テンプレート関数の問題

1. **環境変数が設定されていない**
   - `must_env` 関数で指定された環境変数が設定されていない

   **解決策**: 必要な環境変数が設定されていることを確認します。

2. **テンプレートの構文エラー**
   - JSONやYAMLの構文が正しくない

   **解決策**: `ecspresso render` コマンドを使用して、テンプレートが正しくレンダリングされることを確認します。

## デバッグ方法

1. **デバッグログの有効化**
   ```
   ecspresso --debug コマンド
   ```

2. **差分の確認**
   ```
   ecspresso diff
   ```

3. **設定の検証**
   ```
   ecspresso verify
   ```

4. **テンプレートのレンダリング**
   ```
   ecspresso render
   ```

## サポートリソース

問題が解決しない場合は、以下のリソースを参照してください：

- [GitHub Issues](https://github.com/kayac/ecspresso/issues)
- [ecspresso handbook](https://zenn.dev/fujiwara/books/ecspresso-handbook-v2) (日本語)
