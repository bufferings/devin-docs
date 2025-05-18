---
layout: default
title: version
parent: コマンドリファレンス
nav_order: 17
---

# version

`version`コマンドは、ecspressoのバージョン情報を表示します。

## 基本的な使い方

```bash
ecspresso version
```

## 表示される情報

`version`コマンドは、以下の情報を表示します：

1. **バージョン番号** - ecspressoのバージョン（例：`v1.5.0`）
2. **ビルド情報** - ビルド日時やコミットハッシュ
3. **Go バージョン** - ecspressoのビルドに使用されたGoのバージョン

## 出力例

```
ecspresso v1.5.0
build: 2023-01-01T12:00:00+0000 (commit: abcdef123456)
Go: go1.19
```

## 使用例

### バージョン情報の表示

```bash
ecspresso version
```

### バージョン番号のみを取得

シェルスクリプトでバージョン番号のみを取得する例：

```bash
VERSION=$(ecspresso version | head -n 1 | cut -d ' ' -f 2)
echo "Using ecspresso $VERSION"
```

## 注意事項

- `version`コマンドは、AWS APIを呼び出さないため、AWS認証情報が設定されていなくても実行できます。
- バージョン情報は、バグ報告やサポート依頼の際に役立ちます。
