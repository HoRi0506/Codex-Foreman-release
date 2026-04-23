# Codex-Cli-Captain-Release

[English](./README.md) | [한국어](./README.ko.md) | [日本語](./README.ja.md)

Codex CLI 用の Codex-Cli-Captain インストールリポジトリです。

現在の公開バージョン: `0.0.3`.

`$cap` は Codex を captain として動かし、`ccc-config.toml` のモデルと reasoning 設定に基づいて適切なエージェントへ作業をルーティングします。

## インストール

Codex CLI に次を貼り付けてください。

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
curl -fsSL https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.sh | bash

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## 設定変更の反映

`~/.config/foreman/ccc-config.toml` を編集した後、Codex CLI に次を貼り付けてください。

```text
Run:
ccc setup

Then fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## 推奨ロール設定

| CCC role | Agent | 推奨モデル | Reasoning | 用途 |
| --- | --- | --- | --- | --- |
| `orchestrator` | `captain` | `gpt-5.4` | `high` | LongWay 管理と最終ルーティング判断 |
| `way` | `tactician` | `gpt-5.4` | `medium` | 計画と次の作業選択 |
| `explorer` | `scout` | `gpt-5.4-mini` | `medium` | 読み取り専用の repo 調査 |
| `code specialist` | `raider` | `gpt-5.3-codex` | `high` | コード/config の変更と修復 |
| `documenter` | `scribe` | `gpt-5.4-mini` | `medium` | README、リリースノート、利用者向け文言 |
| `verifier` | `arbiter` | `gpt-5.4` | `medium` | レビュー、リスク、回帰確認 |
| `companion_reader` | `companion_reader` | `gpt-5.4-mini` | `medium` | 低コストの filesystem/docs/web/git/gh 読み取り作業 |
| `companion_operator` | `companion_operator` | `gpt-5.4-mini` | `medium` | 低コストの git/gh 変更と狭い tool 実行 |

## 含まれるもの

- `ccc` CLI と MCP entrypoint
- compact `$cap` skill
- `~/.config/foreman/ccc-config.toml`
- `ccc-config.toml` から同期される CCC-managed custom agents
- 韓国語、英語、日本語、中国語のリクエスト信号に対するルーティング確認
- 軽い filesystem/docs/fetch/git/gh 作業を `gpt-5.4-mini` companion ロールへルーティング
- 強化された raider のモジュール化原則
- compact prompt と `--text`, `--quiet`, `--json-file` の低ノイズ CLI surface
- raw usage event がある場合の token 合計と agent 別ゲージ、ない場合の明確な unavailable 理由
- 最小 release repo、stripped binary、sensitive string scan によるリリース衛生

## 正常確認

```text
CCC install check: status=ok version=0.0.3 entry=$cap registration=matching_registration config=present skill=matching_install
```
