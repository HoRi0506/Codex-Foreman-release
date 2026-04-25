---
name: cap
description: Enter the current user request through Codex-Cli-Captain so host Codex runs the 0.0.4 captain-first LongWay loop instead of answering locally.
metadata:
  short-description: Captain LongWay loop entry
---

# $cap

Use when the operator invokes `$cap`. Remove the literal token and route the remaining request through CCC.

## Contract

Flow: `captain -> Way -> captain -> specialist -> captain -> ... -> end`.

- Host Codex is the public `captain`; CCC owns persisted LongWay, task-cards, routing, lifecycle, visibility, fan-in, validation, review, fallback truth, and commit boundaries.
- Specialists are internal executors selected from `ccc-config.toml`. Prefer CCC-managed custom subagents over detached `codex exec`; use `codex exec` only after an explicit terminal lifecycle checkpoint or recorded fallback reason allows it.
- `/agent` is inspection/thread switching only, not the orchestrator. Do not let one specialist hand off directly to another; every result returns to captain.
- For ordinary `$cap`, use the local `ccc` CLI. `mcp__ccc__...` tools are diagnostics-only unless the operator asks for them.
- Keep stored LongWay text English; answer the operator in their language.

## Start Loop

Use compact CLI surfaces. Full JSON/status is debug-only.

1. Ensure meaningful request text remains after `$cap`; otherwise ask one concise clarification.
2. Verify workspace write support once with `test -w .`. If blocked, state CCC needs workspace-write, then stop or perform one visible read-only fallback only when the request is read-only.
3. Start one bounded Way checkpoint:
   `ccc start --quiet --json '{"prompt":"<request>","title":"<short>","intent":"<short>","goal":"<short>","scope":"<bounded>","acceptance":"<done when>","task_kind":"way","compact":true}'`
4. Make decisions from compact status:
   `ccc status --text --json '{"run_id":"<run_id>"}'`
5. Use compact `command_templates`; avoid `ccc ... --help`, broad `rg`, and history searches for syntax.
6. Prefer `--json-file` for repeated lifecycle payloads.

## Routing

Use CCC-managed custom agents when available:

- `ccc_tactician`: Way/planning
- `ccc_scout`: read-only repo/workspace evidence
- `ccc_raider`: bounded code/config mutation
- `ccc_scribe`: docs/operator text
- `ccc_arbiter`: optional risk/acceptance review
- `ccc_sentinel`: ownership/path classification
- `ccc_companion_reader`: lightweight read-only tool-routed work, including git/`gh` reads
- `ccc_companion_operator`: lightweight mutation/operator-side work, including git/`gh` mutations

Do not route to generic helpers when a matching CCC specialist exists. Direct captain work is limited to explicit fallback, genuinely trivial operator-side fixes, or recorded CCC degradation. When a route-backed companion owner is selected, keep the work out of captain unless a fallback/degradation reason is recorded.

Parallel lanes stay bounded: scouts default to 2 read-only lanes when useful, max 4; raiders default to 2 mutation lanes for broad or multi-file work, max 4; single-file or shared-scope mutation stays sequential.

## Active Specialist Guardrails

While a specialist is active or awaiting fan-in, captain must not drift into doing the specialist's job:

- Do not perform broad repo inspection, implementation, validation, review, or checklist completion locally.
- Do not spawn extra reviewers/specialists unless compact status/replan explicitly requires it and resource pressure is absent.
- Do not merge stale or late specialist output unless captain explicitly accepts it into the chosen path.
- Operator input during active work is captain-owned intervention input only: record the bounded delta/rationale, classify it, and choose one action: same-worker amend, reclaim, or reassignment.
- If host cancellation is unsupported, mark stale work as reclaimed/merged as appropriate and continue from the latest captain decision.
- Under file-handle pressure such as `Too many open files (os error 24)`, open no more agents; terminally record active work, close spawned host agents, and continue single-path.

## Lifecycle

Spawn from compact status `subagent_spawn_contract`: use the named custom agent, avoid full-history fork, and omit agent/model/reasoning overrides already defined by that custom agent.

Record lifecycle with CLI, not MCP:
`ccc subagent-update --quiet --json '{...,"compact":true}'`

- Required order: `spawned -> completed|failed|stalled|reclaimed -> merged`.
- Set `child_agent_id` to the CCC custom agent name (`ccc_raider`, `ccc_scout`, `ccc_arbiter`, etc.); put raw host session identifiers in `thread_id` when useful.
- Do not claim CCC controls host Codex `/agent` Spawned/Waiting row labels.
- Fan-in must stay compact and structured: `summary`, `status`, `evidence_paths`, `next_action`, `open_questions`, `confidence`.
- If fan-in is unsatisfactory, record rationale and next action, then send exactly one narrowed repair or reassignment.
- After every terminal fan-in, close that host agent before opening another, especially under file-handle pressure.

Quiet lifecycle and status output already include token visibility fields or an explicit unavailable reason. Prefer those fields over ad-hoc token guesses.

## Replan And Completion

Replan or resolve through compact orchestrate templates:
`ccc orchestrate --quiet --json '{...,"compact":true}'`

- If `codex_exec_fallback_allowed=false`, do not call `ccc orchestrate` as fallback for that task-card until a terminal subagent update or explicit `fallback_reason` is recorded.
- On stalls, prefer CCC reclaim, retry, or reassign before degraded host-local fallback.
- Use `ccc activity` only for diagnostics when compact status lacks necessary truth.
- `ccc_arbiter` is optional; use it for real risk, ambiguity, failed tests, or release-critical judgment.
- For release note, plan, checklist, `.md`, "finish", "continue", or "끝까지" requests, treat the referenced document as completion criteria. Continue until every in-scope item is completed, explicitly deferred, or blocked; a first bounded slice is not complete.
- If the operator references external paths outside the workspace, keep exact readable paths in scope; if blocked, report the exact path and required approval.
- Reply only when complete, blocked, or waiting on a real operator decision. Do not print the final answer twice.
