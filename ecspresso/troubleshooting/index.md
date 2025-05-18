---
layout: default
title: トラブルシューティング
parent: ecspresso
has_children: false
nav_order: 5
---

# トラブルシューティング

このセクションでは、ecspressoを使用する際に発生する可能性のある一般的な問題とその解決方法について説明します。

## 目次

1. [一般的なエラーと解決策](#一般的なエラーと解決策)
2. [デプロイ関連の問題](#デプロイ関連の問題)
3. [タスク定義関連の問題](#タスク定義関連の問題)
4. [サービス定義関連の問題](#サービス定義関連の問題)
5. [AWS認証関連の問題](#aws認証関連の問題)
6. [よくある質問](#よくある質問)
7. [サポートを受ける方法](#サポートを受ける方法)

## 一般的なエラーと解決策

### エラー: 設定ファイルが見つかりません

```
Error: open ecspresso.yml: no such file or directory
```

**解決策**:
- 正しいディレクトリで実行しているか確認してください
- `--config`オプションで正しい設定ファイルのパスを指定してください
- `ecspresso init`コマンドを実行して、新しい設定ファイルを作成してください

### エラー: AWS認証情報が見つかりません

```
Error: NoCredentialProviders: no valid providers in chain
```

**解決策**:
- AWS CLIの設定が正しいか確認してください
- 環境変数`AWS_ACCESS_KEY_ID`と`AWS_SECRET_ACCESS_KEY`が設定されているか確認してください
- IAMロールが正しく設定されているか確認してください

### エラー: リージョンが見つかりません

```
Error: MissingRegion: could not find region configuration
```

**解決策**:
- 環境変数`AWS_REGION`または`AWS_DEFAULT_REGION`を設定してください
- `~/.aws/config`ファイルにリージョンが設定されているか確認してください
- 設定ファイル`ecspresso.yml`に`region`が指定されているか確認してください

### エラー: タイムアウト

```
Error: context deadline exceeded
```

**解決策**:
- `--timeout`オプションでタイムアウト時間を長く設定してください
- ネットワーク接続を確認してください
- AWS APIの制限に達していないか確認してください

## デプロイ関連の問題

### エラー: サービスが見つかりません

```
Error: service not found: myservice
```

**解決策**:
- サービス名が正しいか確認してください
- クラスター名が正しいか確認してください
- リージョンが正しいか確認してください
- サービスが存在するか確認してください（初回デプロイの場合は`create`コマンドを使用）

### エラー: タスク定義の登録に失敗

```
Error: failed to register task definition: InvalidParameterException: Invalid setting for container
```

**解決策**:
- タスク定義ファイルの構文を確認してください
- 必須パラメータが設定されているか確認してください
- コンテナ定義が正しいか確認してください
- `ecspresso verify`コマンドを使用して、タスク定義を検証してください

### エラー: サービスの更新に失敗

```
Error: failed to update service: InvalidParameterException: Unable to update task definition
```

**解決策**:
- サービス定義ファイルの構文を確認してください
- 必須パラメータが設定されているか確認してください
- サービスの状態を確認してください（`ecspresso status`）
- 前回のデプロイが完了しているか確認してください

### エラー: デプロイメントの失敗

```
Error: deployment failed: DEPLOYMENT_FAILED
```

**解決策**:
- サービスのイベントを確認してください（`ecspresso status`）
- コンテナのログを確認してください
- ヘルスチェックの設定を確認してください
- タスク定義が正しいか確認してください
- `--rollback-events`オプションを使用して、自動的にロールバックするように設定してください

## タスク定義関連の問題

### エラー: コンテナ定義が無効

```
Error: InvalidParameterException: Invalid container definition: essential must be true for at least one container in a task definition.
```

**解決策**:
- 少なくとも1つのコンテナで`essential: true`を設定してください
- コンテナ定義の構文を確認してください
- 必須パラメータが設定されているか確認してください

### エラー: メモリ制限が無効

```
Error: InvalidParameterException: Memory limit must be specified for container.
```

**解決策**:
- コンテナに`memory`または`memoryReservation`を設定してください
- タスク定義に`memory`を設定してください
- Fargateの場合は、有効なCPUとメモリの組み合わせを使用してください

### エラー: ポートマッピングが無効

```
Error: InvalidParameterException: Invalid 'portMappings' for container: hostPort must be specified or set to 0 for container.
```

**解決策**:
- `hostPort`を指定するか、0に設定してください
- `networkMode`が`awsvpc`の場合は、`hostPort`と`containerPort`を同じ値に設定してください
- ポートマッピングの構文を確認してください

### エラー: イメージが見つかりません

```
Error: CannotPullContainerError: The container image does not exist or the provided credentials do not have permissions to access it.
```

**解決策**:
- イメージ名が正しいか確認してください
- イメージがリポジトリに存在するか確認してください
- 認証情報が正しいか確認してください
- プライベートリポジトリの場合は、`repositoryCredentials`を設定してください

## サービス定義関連の問題

### エラー: サブネットが無効

```
Error: InvalidParameterException: The subnet ID 'subnet-12345678' does not exist
```

**解決策**:
- サブネットIDが正しいか確認してください
- サブネットが指定したリージョンに存在するか確認してください
- VPCの設定を確認してください

### エラー: セキュリティグループが無効

```
Error: InvalidParameterException: The security group ID 'sg-12345678' does not exist
```

**解決策**:
- セキュリティグループIDが正しいか確認してください
- セキュリティグループが指定したリージョンに存在するか確認してください
- VPCの設定を確認してください

### エラー: ロードバランサーが無効

```
Error: InvalidParameterException: The target group ARN 'arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890' does not exist
```

**解決策**:
- ターゲットグループARNが正しいか確認してください
- ターゲットグループが指定したリージョンに存在するか確認してください
- ロードバランサーの設定を確認してください

### エラー: デプロイメントコントローラーが無効

```
Error: InvalidParameterException: Deployment controller type 'CODE_DEPLOY' requires a load balancer configuration.
```

**解決策**:
- Blue/Greenデプロイメントを使用する場合は、ロードバランサーを設定してください
- `deploymentController`の設定を確認してください
- CodeDeployの設定を確認してください

## AWS認証関連の問題

### エラー: アクセス拒否

```
Error: AccessDeniedException: User: arn:aws:iam::123456789012:user/myuser is not authorized to perform: ecs:DescribeServices
```

**解決策**:
- IAMユーザーまたはロールに必要な権限が付与されているか確認してください
- 必要な権限のリストについては、[AWS ECSのドキュメント](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/security_iam_service-with-iam.html)を参照してください
- 一時的な認証情報が有効期限切れでないか確認してください

### エラー: リソースが見つかりません

```
Error: ResourceNotFoundException: The specified resource was not found.
```

**解決策**:
- リソース名またはARNが正しいか確認してください
- リソースが指定したリージョンに存在するか確認してください
- リソースが削除されていないか確認してください

### エラー: スロットリング

```
Error: ThrottlingException: Rate exceeded
```

**解決策**:
- リクエストの頻度を下げてください
- AWS APIの制限に達している場合は、制限の引き上げをリクエストしてください
- 再試行ロジックを実装してください

## よくある質問

### Q: ecspressoはどのようにAWS認証情報を取得しますか？

A: ecspressoは、AWS SDKの標準的な認証情報プロバイダーチェーンを使用します。以下の順序で認証情報を検索します：

1. 環境変数（`AWS_ACCESS_KEY_ID`、`AWS_SECRET_ACCESS_KEY`、`AWS_SESSION_TOKEN`）
2. 共有認証情報ファイル（`~/.aws/credentials`）
3. EC2インスタンスプロファイルまたはECSタスクロール

### Q: ecspressoはどのようにリージョンを決定しますか？

A: ecspressoは、以下の順序でリージョンを決定します：

1. 設定ファイル（`ecspresso.yml`）の`region`パラメータ
2. 環境変数（`AWS_REGION`または`AWS_DEFAULT_REGION`）
3. 共有設定ファイル（`~/.aws/config`）の`region`パラメータ

### Q: タスク定義のリビジョンはどのように管理されますか？

A: タスク定義のリビジョンは、AWS ECSによって自動的に管理されます。新しいタスク定義を登録するたびに、リビジョン番号が増加します。ecspressoは、最新のリビジョンを使用してサービスをデプロイします。

### Q: Blue/Greenデプロイメントを使用するにはどうすればよいですか？

A: Blue/Greenデプロイメントを使用するには、以下の設定が必要です：

1. サービス定義ファイルで`deploymentController.type`を`CODE_DEPLOY`に設定
2. ロードバランサーを設定
3. `ecspresso.yml`ファイルで`codedeploy`セクションを設定
4. `appspec.yml`ファイルを作成

詳細については、[大規模サービスの管理](../practical/large-scale.html#bluegreenデプロイメント)を参照してください。

### Q: ecspressoはFargateスポットをサポートしていますか？

A: はい、ecspressoはFargateスポットをサポートしています。サービス定義ファイルで`capacityProviderStrategy`を設定することで、Fargateスポットを使用できます。詳細については、[大規模サービスの管理](../practical/large-scale.html#fargateスポットの使用)を参照してください。

### Q: ecspressoはECS Service Connectをサポートしていますか？

A: はい、ecspressoはECS Service Connectをサポートしています。サービス定義ファイルで`serviceConnectConfiguration`を設定することで、ECS Service Connectを使用できます。詳細については、[大規模サービスの管理](../practical/large-scale.html#ecs-service-connect)を参照してください。

### Q: ecspressoはVPC Latticeをサポートしていますか？

A: はい、ecspressoはVPC Latticeをサポートしています。サービス定義ファイルで`serviceConnectConfiguration`を設定し、`vpcLatticeConfiguration`を追加することで、VPC Latticeを使用できます。

## サポートを受ける方法

ecspressoに関する問題やフィードバックがある場合は、以下の方法でサポートを受けることができます：

1. [GitHub Issues](https://github.com/kayac/ecspresso/issues)で問題を報告する
2. [GitHub Discussions](https://github.com/kayac/ecspresso/discussions)で質問する
3. [GitHub Pull Requests](https://github.com/kayac/ecspresso/pulls)で改善を提案する

また、ecspressoのソースコードは[GitHub](https://github.com/kayac/ecspresso)で公開されているため、自分で問題を解決することもできます。
