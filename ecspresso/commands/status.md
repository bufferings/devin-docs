---
layout: default
title: status
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 6
---

# status

`status`コマンドは、ECSサービスの現在の状態を表示するためのコマンドです。デプロイメント状況、実行中のタスク数、最近のイベントなどの情報を確認できます。

## 基本的な使い方

```console
$ ecspresso status [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|------------|
| `--config` | 設定ファイルのパス | `ecspresso.yml` |
| `--events` | 表示するイベント数 | `10` |

## 出力情報

`status`コマンドは、以下の情報を表示します：

1. **サービス基本情報**
   - サービス名
   - クラスター名
   - タスク定義ARN

2. **デプロイメント情報**
   - デプロイメントID
   - タスク定義リビジョン
   - 希望タスク数
   - 保留中タスク数
   - 実行中タスク数

3. **イベント情報**
   - 最近のサービスイベント（デフォルトで10件）

## 出力例

```
Service: myservice
Cluster: default
TaskDefinition: myservice:3
Deployments:
    PRIMARY myservice:3 desired:2 pending:0 running:2
Events:
2023/01/01 12:00:00 (service myservice) has reached a steady state.
2023/01/01 11:58:00 (service myservice) has started 2 tasks: (task 11111111-1111-1111-1111-111111111111) (task 22222222-2222-2222-2222-222222222222).
2023/01/01 11:57:00 (service myservice) registered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890)
2023/01/01 11:56:00 (service myservice) has begun draining connections on 2 tasks.
2023/01/01 11:55:00 (service myservice) deregistered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890)
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso status --config ecspresso.yml
```

### より多くのイベントを表示

```console
$ ecspresso status --config ecspresso.yml --events 20
```

### 特定の環境の設定ファイルを使用

```console
$ ecspresso status --config ecspresso.prod.yml
```

## ステータス情報の解釈

### デプロイメント状態

デプロイメント情報は、以下の形式で表示されます：

```
PRIMARY myservice:3 desired:2 pending:0 running:2
```

- **PRIMARY**: デプロイメントタイプ（通常は`PRIMARY`）
- **myservice:3**: タスク定義名とリビジョン
- **desired:2**: 希望タスク数
- **pending:0**: 保留中のタスク数
- **running:2**: 実行中のタスク数

理想的な状態では、`pending:0`かつ`desired`と`running`の値が一致している状態です。

### イベントの種類

主なイベントの種類と意味：

1. **steady state**
   - サービスが安定状態に達したことを示します
   - 例: `(service myservice) has reached a steady state.`

2. **task started**
   - 新しいタスクが開始されたことを示します
   - 例: `(service myservice) has started 2 tasks: (task 11111111-1111-1111-1111-111111111111) (task 22222222-2222-2222-2222-222222222222).`

3. **target registered**
   - ターゲットグループにターゲットが登録されたことを示します
   - 例: `(service myservice) registered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890)`

4. **connection draining**
   - タスクの接続ドレイニングが開始されたことを示します
   - 例: `(service myservice) has begun draining connections on 2 tasks.`

5. **target deregistered**
   - ターゲットグループからターゲットが登録解除されたことを示します
   - 例: `(service myservice) deregistered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-1:123456789012:targetgroup/myservice/abcdef1234567890)`

6. **deployment failed**
   - デプロイメントが失敗したことを示します
   - 例: `(service myservice) failed to deploy a new task definition.`

## 関連コマンド

- [deploy](./deploy.html) - サービスをデプロイ
- [tasks](./tasks.html) - タスクの一覧を表示
- [wait](./wait.html) - サービスが安定状態になるまで待機
- [diff](./diff.html) - ローカルの設定ファイルと現在のサービスとの差分を表示
