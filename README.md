# Codex-Cli-Captain-Release

[English](./README.md) | [한국어](./README.ko.md) | [日本語](./README.ja.md)

Install Codex-Cli-Captain for Codex CLI.

Current public release: `0.0.3`.

Use `$cap` when you want Codex to act as captain, route work across the right agents, and use the models and reasoning levels from `ccc-config.toml`.

## Install

Copy this into Codex CLI:

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
curl -fsSL https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.sh | bash

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## Reapply Config Changes

After editing `~/.config/foreman/ccc-config.toml`, paste this into Codex CLI:

```text
Run:
ccc setup

Then fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## Recommended Role Defaults

| CCC role | Agent | Recommended model | Reasoning | Notes |
| --- | --- | --- | --- | --- |
| `orchestrator` | `captain` | `gpt-5.4` | `high` | LongWay ownership and final routing judgment |
| `way` | `tactician` | `gpt-5.4` | `medium` | Planning and bounded next-move selection |
| `explorer` | `scout` | `gpt-5.4-mini` | `medium` | Read-only repo evidence |
| `code specialist` | `raider` | `gpt-5.3-codex` | `high` | Code/config mutation and repair |
| `documenter` | `scribe` | `gpt-5.4-mini` | `medium` | README, release notes, and operator text |
| `verifier` | `arbiter` | `gpt-5.4` | `medium` | Review, risk, regression, and acceptance checks |
| `companion_reader` | `companion_reader` | `gpt-5.4-mini` | `medium` | Low-cost filesystem/docs/web/git/gh read work |
| `companion_operator` | `companion_operator` | `gpt-5.4-mini` | `medium` | Low-cost bounded git/gh mutation and narrow tool work |

## What You Get

- `ccc` CLI and MCP entrypoint
- compact `$cap` skill
- `~/.config/foreman/ccc-config.toml`
- CCC-managed custom agents synced from `ccc-config.toml`
- multilingual routing checks for Korean, English, Japanese, and Chinese request signals
- runtime companion routing for lightweight filesystem/docs/fetch/git/gh work through `gpt-5.4-mini` companion roles
- stronger raider modularization guidance
- compact captain/subagent prompts and lower-noise `--text`, `--quiet`, and `--json-file` surfaces
- token totals and a stacked gauge when raw delegated-worker usage events are available; explicit unavailable reasons when host custom subagent usage is not exposed
- release hardening with a minimal release repo, stripped release binary, and sensitive-string scan

## Healthy Check

```text
CCC install check: status=ok version=0.0.3 entry=$cap registration=matching_registration config=present skill=matching_install
```

## Release Note

- [`docs/release/notes/v0.0.3.md`](./docs/release/notes/v0.0.3.md): current public release card body
