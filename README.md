# Codex-Cli-Captain

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.ko.md">한국어</a> ·
  <a href="./README.ja.md">日本語</a>
</p>

<p align="center">
  <img src="./docs/assets/ccc-banner.png" alt="CCC Codex-Cli-Captain banner" width="100%">
</p>

<p align="center"><em>Want to run Codex CLI or Codex App end-to-end?<br>
Worried about running the whole thing on high-end models?<br>
Then how about using CCC?<br>
Just put <code>$cap</code> in front of what you want to do.<br>
Then something remarkable can unfold.</em></p>

Current public release: `0.0.13-pre`.

Supported platforms: `darwin-arm64`, `darwin-x86_64`, `linux-arm64`, `linux-x86_64`, `windows-x86_64`.

## Install

macOS or Linux:

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
curl -fsSL https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.sh | bash

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

Windows PowerShell:

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
iwr -UseB https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.ps1 | iex

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

To update, run the same install command again, restart Codex CLI, then run `ccc check-install`.

## Basic Use

Start a CCC-managed task by adding `$cap` before your request:

```text
$cap Refactor the auth flow and keep tests passing
```

CCC treats `$cap` as the entrypoint. It owns the LongWay, task cards, checklist, fan-in, status, and restart handoff for the work.

## v0.0.13-pre Behavior

- `$cap` is the public entrypoint.
- Operator-visible lifecycle changes should prefer quiet `ran` commands such as `ccc start --quiet --json-file`, `ccc orchestrate --quiet --json-file`, `ccc subagent-update --quiet --json-file`, `ccc memory --quiet --json-file`, `ccc checklist --quiet --json-file`, and `ccc status --quiet --json-file`; `ccc status --text --json-file` is the compact transcript path.
- `ccc checklist` and `ccc status` should stay concise. LongWay rows are short operator-facing summaries.
- Configured `ccc_*` custom agents are the default specialist targets. Generic `worker` and `explorer` labels are invalid unless the operator explicitly overrides them.
- `ccc memory` is opt-in and unconfigured by default.
- The SSL Skill Registry is available as bounded evidence for routing, planning, and review; it does not replace persisted run state.
- Mutation completion waits for specialist fan-in, and arbiter review remains the final gate for review-sensitive changes.

## Planning

`$cap` is the CCC entrypoint. Host planning surfaces are not CCC orchestration paths:

- Invoke `$cap` for broad, risky, or ambiguous work so CCC creates a `PLAN_SEQUENCE` and routes planning to the configured Way agent.
- Host Plan Mode can help frame what you type, but CCC does not trigger it inside Way as a background planning engine.
- CCC LongWay, task cards, checklist, fan-in, and status remain the source of truth.
- For narrow work, `$cap` alone is enough.

Example:

```text
$cap Plan the release README update, ask any blocking Way questions first, and wait for LongWay approval before changing files
```

## Recommended Role Defaults

For regular CCC use, ChatGPT Pro $100 is the recommended starting plan because `$cap` workflows can spend more Codex usage through repeated captain and specialist handoffs. Adjust reasoning by your working style and task risk: keep higher reasoning for broad planning, risky code changes, or reviews, and lower it for narrow, repetitive, or low-risk tasks.

| CCC role | Agent | Recommended model | Reasoning | Notes |
| --- | --- | --- | --- | --- |
| `orchestrator` | `captain` | `gpt-5.5` | `medium` | LongWay ownership and final routing judgment |
| `way` | `tactician` | `gpt-5.5` | `high` | Planning and bounded next-move selection |
| `explorer` | `scout` | `gpt-5.4-mini` | `high` | Read-only repo evidence |
| `code specialist` | `raider` | `gpt-5.5` | `high` | Code/config mutation and repair |
| `documenter` | `scribe` | `gpt-5.4-mini` | `medium` | README, release notes, and operator text |
| `verifier` | `arbiter` | `gpt-5.5` | `high` | Review, risk, regression, and acceptance checks |
| `companion_reader` | `companion_reader` | `gpt-5.4-mini` | `medium` | Low-cost filesystem/docs/web/git/gh read work |
| `companion_operator` | `companion_operator` | `gpt-5.4-mini` | `medium` | Low-cost bounded git/gh mutation and narrow tool work |

`gpt-5.5` is recommended for the high-value roles when Codex is signed in with ChatGPT. If it is unavailable in the current account or execution path, use `gpt-5.4` for those roles until rollout reaches that path.
