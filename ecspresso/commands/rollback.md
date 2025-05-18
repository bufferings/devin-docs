---
layout: default
title: rollback
parent: コマンドリファレンス
nav_order: 12
---

# rollback

`rollback`コマンドは、サービスを以前のタスク定義にロールバックします。

## 使い方

```console
$ ecspresso rollback --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--revision` | ロールバック先のタスク定義リビジョン |
| `--deregister` | ロールバック後に現在のタスク定義を登録解除 |
| `--wait-until-stable` | サービスが安定するまで待機（デフォルト: true） |
| `--no-wait-until-stable` | サービスが安定するまで待機しない |
| `--dry-run` | 実際にロールバックを行わずに実行内容を表示 |

## 使用例

### 基本的な使用方法（直前のリビジョンにロールバック）

```console
$ ecspresso rollback --config ecspresso.yml
```

### 特定のリビジョンにロールバック

```console
$ ecspresso rollback --config ecspresso.yml --revision 10
```

### ロールバック後に現在のタスク定義を登録解除

```console
$ ecspresso rollback --config ecspresso.yml --deregister
```

### ドライランモードで実行内容を確認

```console
$ ecspresso rollback --config ecspresso.yml --dry-run
```

## ロールバックフロー

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Ecspresso as ecspresso
    participant ECS as Amazon ECS
    
    User->>Ecspresso: rollback コマンド実行
    Ecspresso->>ECS: 現在のサービス情報を取得
    ECS-->>Ecspresso: サービス情報
    Ecspresso->>ECS: タスク定義リビジョンを取得
    ECS-->>Ecspresso: タスク定義リビジョン一覧
    
    alt リビジョン指定あり
        Ecspresso->>Ecspresso: 指定されたリビジョンを使用
    else リビジョン指定なし
        Ecspresso->>Ecspresso: 直前のリビジョンを使用
    end
    
    Ecspresso->>ECS: サービスを更新
    ECS-->>Ecspresso: サービス更新開始
    
    alt wait-until-stable=true
        Ecspresso->>ECS: サービスの安定を待機
        ECS-->>Ecspresso: サービス安定通知
    end
    
    alt deregister=true
        Ecspresso->>ECS: 現在のタスク定義を登録解除
        ECS-->>Ecspresso: 登録解除完了
    end
    
    Ecspresso-->>User: ロールバック完了
```

## 注意事項

- リビジョンを指定しない場合、直前のリビジョンにロールバックされます。
- ロールバック先のタスク定義が存在しない場合、エラーが発生します。
- CodeDeployを使用したBlue/Greenデプロイメントの場合、このコマンドは使用できません。代わりにCodeDeployのロールバック機能を使用してください。
