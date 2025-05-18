---
layout: default
title: render
nav_order: 10
parent: コマンドリファレンス
grand_parent: ecspresso
---

# render

`render`コマンドは、設定、サービス定義、またはタスク定義ファイルをSTDOUTにレンダリングします。テンプレート変数が展開され、最終的な設定を確認できます。

## 構文

```
ecspresso render [オプション]
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
| `--config-only` | 設定ファイルのみをレンダリング | `false` |
| `--task-definition-only` | タスク定義のみをレンダリング | `false` |
| `--service-definition-only` | サービス定義のみをレンダリング | `false` |
| `--appspec-only` | AppSpecのみをレンダリング | `false` |
| `--format` | 出力形式（json/yaml） | `json` |

## 使用例

### 基本的な使用方法（すべての設定をレンダリング）

```bash
ecspresso render
```

### タスク定義のみをレンダリング

```bash
ecspresso render --task-definition-only
```

### サービス定義のみをレンダリング

```bash
ecspresso render --service-definition-only
```

### YAML形式で出力

```bash
ecspresso render --format yaml
```

## テンプレート機能

`render`コマンドは、設定ファイル内のテンプレート変数を展開します。ecspressoは以下のテンプレート機能をサポートしています：

1. 環境変数の参照
```json
{
  "containerDefinitions": [
    {
      "name": "app",
      "image": "{{ env `IMAGE_NAME` }}"
    }
  ]
}
```

2. AWS Systems Manager Parameter Storeの参照
```json
{
  "containerDefinitions": [
    {
      "name": "app",
      "image": "{{ ssm `/myapp/image_name` }}"
    }
  ]
}
```

3. AWS Secrets Managerの参照
```json
{
  "containerDefinitions": [
    {
      "secrets": [
        {
          "name": "API_KEY",
          "valueFrom": "{{ secretsmanager_arn `myapp/api_key` }}"
        }
      ]
    }
  ]
}
```

## Jsonnetサポート

ecspressoはJsonnetテンプレート言語もサポートしています。Jsonnetを使用するには、ファイル拡張子を`.jsonnet`にし、外部変数を渡します：

```bash
ecspresso --ext-str env=production render
```

## 関連コマンド

- [verify](./verify.html) - 設定内のリソースを検証
- [diff](./diff.html) - タスク定義、サービス定義と実行中のサービス間の差分を表示
