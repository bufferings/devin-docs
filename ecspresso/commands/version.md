---
layout: default
title: version
parent: コマンドリファレンス
grand_parent: ecspresso
nav_order: 17
---

# version

`version`コマンドは、ecspressoのバージョン情報を表示するためのコマンドです。インストールされているecspressoのバージョンを確認する場合に使用します。

## 基本的な使い方

```console
$ ecspresso version
```

## 出力例

```
ecspresso v2.3.0
```

## 使用例

### 基本的な使用方法

```console
$ ecspresso version
```

### バージョン情報をファイルに保存

```console
$ ecspresso version > version.txt
```

### バージョン情報を変数に格納

```bash
VERSION=$(ecspresso version | cut -d ' ' -f 2)
echo "Using ecspresso $VERSION"
```

## バージョン管理

ecspressoは、セマンティックバージョニングに従ってバージョン管理されています。バージョン番号は、`vX.Y.Z`の形式で表されます。

- **X**: メジャーバージョン（互換性のない変更）
- **Y**: マイナーバージョン（後方互換性のある機能追加）
- **Z**: パッチバージョン（後方互換性のあるバグ修正）

## バージョンの確認方法

ecspressoのバージョンを確認するには、以下の方法があります：

1. `version`コマンドを使用する
2. `--version`オプションを使用する
3. `-v`オプションを使用する

```console
$ ecspresso version
ecspresso v2.3.0

$ ecspresso --version
ecspresso v2.3.0

$ ecspresso -v
ecspresso v2.3.0
```

## バージョンの更新

ecspressoの最新バージョンに更新するには、インストール方法に応じて以下の手順を実行します：

### Homebrew（macOS）

```console
$ brew update
$ brew upgrade ecspresso
```

### Go

```console
$ go install github.com/kayac/ecspresso/v2@latest
```

### バイナリ

[GitHub Releases](https://github.com/kayac/ecspresso/releases)から最新のバイナリをダウンロードして、パスの通った場所に配置します。

## CI/CDパイプラインでの使用

`version`コマンドは、CI/CDパイプラインでecspressoのバージョンを確認するのに役立ちます。以下は、GitHub Actionsでの使用例です：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: kayac/ecspresso@v2
        with:
          version: v2.3.0
      - run: |
          # ecspressoのバージョンを確認
          ECSPRESSO_VERSION=$(ecspresso version | cut -d ' ' -f 2)
          echo "Using ecspresso $ECSPRESSO_VERSION"
          
          # バージョンチェック
          if [[ "$ECSPRESSO_VERSION" != "v2.3.0" ]]; then
            echo "Warning: Expected ecspresso v2.3.0, but got $ECSPRESSO_VERSION"
          fi
          
          # デプロイを実行
          ecspresso deploy --config ecspresso.yml
```

## 注意事項

- `version`コマンドは、オプションを必要としません
- バージョン情報は、標準出力に表示されます
- ecspressoの最新バージョンを使用することをお勧めします
- 異なるバージョンのecspressoを使用する場合は、互換性に注意してください
- CI/CDパイプラインでは、特定のバージョンのecspressoを使用することをお勧めします

## 関連コマンド

- [help](./help.html) - ヘルプ情報を表示
