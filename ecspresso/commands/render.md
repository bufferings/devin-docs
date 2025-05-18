---
layout: default
title: render
parent: コマンドリファレンス
nav_order: 10
---

# render

`render`コマンドは、設定、サービス定義またはタスク定義ファイルを標準出力にレンダリングします。テンプレート変数が展開された状態で確認できます。

## 使い方

```console
$ ecspresso render --config ecspresso.yml
```

## オプション

| オプション | 説明 |
|------------|------|
| `--config` | 設定ファイルのパス（デフォルト: ecspresso.yml） |
| `--render-config` | 設定ファイルをレンダリング |
| `--render-service` | サービス定義をレンダリング |
| `--render-task` | タスク定義をレンダリング（デフォルト） |
| `--task-definition` | タスク定義のJSONファイルパス |
| `--service-definition` | サービス定義のJSONファイルパス |

## 使用例

### タスク定義をレンダリング（デフォルト）

```console
$ ecspresso render --config ecspresso.yml
```

### サービス定義をレンダリング

```console
$ ecspresso render --config ecspresso.yml --render-service
```

### 設定ファイルをレンダリング

```console
$ ecspresso render --config ecspresso.yml --render-config
```

### 特定のタスク定義ファイルをレンダリング

```console
$ ecspresso render --config ecspresso.yml --task-definition my-task-def.json
```

## 使用シナリオ

`render`コマンドは、以下のような場合に便利です：

1. テンプレート変数が正しく展開されているか確認したい場合
2. 環境変数の値が正しく反映されているか確認したい場合
3. Jsonnetファイルの出力を確認したい場合
4. デプロイ前に最終的な設定を確認したい場合

## 注意事項

- このコマンドは設定を変更せず、レンダリング結果を標準出力に表示するだけです。
- 環境変数が設定されていない場合、`must_env`関数を使用している部分でエラーが発生します。
