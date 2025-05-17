---
layout: default
title: コマンドリファレンス
parent: ecspresso
nav_order: 3
has_children: true
---

# コマンドリファレンス

ecspressoは以下のコマンドを提供します：

```
Usage: ecspresso <command>

Flags:
  -h, --help                      ヘルプを表示
      --envfile=ENVFILE,...       環境ファイル ($ECSPRESSO_ENVFILE)
      --debug                     デバッグログを有効化 ($ECSPRESSO_DEBUG)
      --ext-str=KEY=VALUE;...     Jsonnet用の外部文字列値 ($ECSPRESSO_EXT_STR)
      --ext-code=KEY=VALUE;...    Jsonnet用の外部コード値 ($ECSPRESSO_EXT_CODE)
      --config="ecspresso.yml"    設定ファイル ($ECSPRESSO_CONFIG)
      --assume-role-arn=""        引き受けるロールのARN ($ECSPRESSO_ASSUME_ROLE_ARN)
      --timeout=TIMEOUT           タイムアウト。設定ファイルで上書き可能 ($ECSPRESSO_TIMEOUT)
      --filter-command=STRING     フィルターコマンド ($ECSPRESSO_FILTER_COMMAND)
      --[no-]color                カラー出力を有効化 ($ECSPRESSO_COLOR)
```

各コマンドの詳細については、サブセクションを参照してください。
