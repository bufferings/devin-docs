---
layout: default
title: version
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 17
---

# version

`version`コマンドは、ecspressoのバージョン情報を表示します。

## 基本的な使い方

```bash
ecspresso version
```

## オプション

| オプション | 説明 | デフォルト値 |
|------------|------|-------------|
| `--output` | 出力形式（text, json） | `text` |

## 詳細

`version`コマンドは、以下の情報を表示します：

1. ecspressoのバージョン
2. ビルド情報（コミットハッシュ、ビルド日時）
3. Go言語のバージョン
4. 実行環境（OS、アーキテクチャ）

このコマンドは、ecspressoのバージョンを確認するのに役立ちます。

## 出力例

### テキスト形式（デフォルト）

```
ecspresso version v2.0.0
build: v2.0.0-1-g1234567 (2023-01-01 00:00:00 +0000 UTC)
go: go1.19
os: linux
arch: amd64
```

### JSON形式

```json
{
  "version": "v2.0.0",
  "build": "v2.0.0-1-g1234567",
  "buildDate": "2023-01-01T00:00:00Z",
  "goVersion": "go1.19",
  "os": "linux",
  "arch": "amd64"
}
```

## 使用例

### 基本的な使用例

```bash
ecspresso version
```

### JSON形式で出力する例

```bash
ecspresso version --output json
```

## バージョン管理

ecspressoは、セマンティックバージョニング（SemVer）に従ってバージョン管理されています。バージョン番号は、「メジャー.マイナー.パッチ」の形式で表されます。

- メジャーバージョン：互換性のない変更が含まれる場合に増加
- マイナーバージョン：後方互換性のある機能追加が含まれる場合に増加
- パッチバージョン：後方互換性のあるバグ修正が含まれる場合に増加

## バージョンの確認方法

ecspressoのバージョンを確認するには、以下のいずれかの方法を使用します：

1. `ecspresso version`コマンドを実行する
2. `ecspresso --version`オプションを使用する
3. `ecspresso -v`オプションを使用する

## バージョンの更新

ecspressoの最新バージョンに更新するには、インストール方法に応じて以下のコマンドを使用します：

### Homebrewの場合

```bash
brew upgrade kayac/tap/ecspresso
```

### asdfの場合

```bash
asdf install ecspresso latest
asdf global ecspresso latest
```

### aquaの場合

```bash
aqua update
aqua install
```

### バイナリの場合

GitHubのリリースページから最新バージョンをダウンロードしてインストールします。

```bash
curl -sL https://github.com/kayac/ecspresso/releases/download/vX.Y.Z/ecspresso-vX.Y.Z-linux-amd64.zip -o ecspresso.zip
unzip ecspresso.zip
sudo mv ecspresso /usr/local/bin/
```

## 関連コマンド

- `help` - ecspressoのヘルプを表示します。
