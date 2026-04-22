---
name: cap
description: Enter the current user request through the installed Codex-Foreman MCP so host Codex/captain can run the 0.0.1 checklist loop instead of answering purely through the host session.
metadata:
  short-description: Captain checklist loop entry
---

# $cap

Use this skill when the operator invokes `$cap` and wants host Codex/captain to route the request through Foreman as a conservative checklist loop.

## Canonical model

The only intended public model is:

1. `captain`
2. `plan(checklist)`
3. `agent 1`
4. `captain(checklist update)`
5. `agent 2`
6. `captain(checklist update)`
7. `...`
8. `captain end`

Do not present `$cap` as a fixed phase chain.

Do not let one specialist hand off directly to the next specialist without a captain checkpoint in between.

Do not treat `$cap` as a hidden worker command surface. The host Codex session acting as `captain` is the public receiver for this skill; the packaged Foreman specialists are internal executors.

## Core rules

- treat the operator's message, minus the literal `$cap` token, as the request that should enter Foreman
- use the current Codex session as the public request boundary; persisted Foreman runs are internal execution state
- for a fresh request, prefer `mcp__codex_foreman__foreman_auto_entry` first
- if auto-entry does not produce a usable run, prefer an explicit Foreman plan/run surface that preserves captain planning; do not fall back to a host-local answer just because the first entry surface degraded
- canonical persisted checklist text must be English
- if the operator asks about checklist status, translate or summarize it in the operator's language at response time without rewriting the stored checklist text
- in normal operation, `captain` does not directly edit code or docs
- actual work should be done by the selected specialist agent
- only allow `captain` direct mutation when Foreman cannot launch the specialist path honestly, bounded retry already failed, or the operator explicitly approves a host-local fallback

## Visibility contract

Prefer the new checklist-loop wording over the older phase wording.

When Foreman exposes compact runtime truth, keep the operator update anchored to lines like:

- `Checklist: 1/3 completed`
- `Tokens: 10.5k used`
- `[████████████████ ███████ ███]`
- `By Agent: raider 72% (7.6k) | captain 28% (2.9k)`
- `Current Item: item-2`
- `Agent Loop: Foreman launched scribe [gpt-5.4-mini/medium]`
- `Spawned: scribe`
- `Changed: captain marked item-1 completed`
- `Next: raider`

Do not invent or prefer `Phase Chain: ...` wording when the checklist-loop wording is available.

Surface `Spawned:` when a worker starts and `Changed:` when captain updates the checklist or the next action changes materially.
When `foreman_status` exposes `token_usage`, every captain checklist block is expected to include a `Tokens:` line in that same checklist block instead of hiding token visibility in a separate Foreman status dump.
Do not omit the `Tokens:` line from the checklist block just because the value did not change on the latest poll.
The operator's quiet-mode suppression applies only to the raw MCP call transcript such as `Called codex-foreman.foreman_status(...)`; it does not apply to the public captain checklist block.
When `token_usage.by_agent` is available, prefer the full visual form instead of a bare number-only token line:

- keep the first line as `Tokens: 10.5k used`
- add a dedicated stacked usage bar line immediately under `Tokens:`, such as `[████████████████ ███████ ███]`
- use contiguous `█` segments in descending share order and separate agent boundaries with single spaces
- add a `By Agent:` line immediately under the bar line
- preserve the per-agent percentage and token count on that line

Prefer the checklist block order:

- `Checklist: ...`
- `Tokens: ...` when available
- `[████ ...]` when `token_usage.by_agent` is available
- `By Agent: ...` when `token_usage.by_agent` is available
- `Current Item: ...`
- `Agent Loop:` or `Spawned:` when materially useful
- `Changed:` when captain updated checklist state
- `Next: ...`

## Token discipline

- prefer `mcp__codex_foreman__foreman_status` first and use `mcp__codex_foreman__foreman_activity` only when status is insufficient for the next decision
- before emitting a captain checklist block, refresh or reuse the latest `foreman_status` truth and carry `token_usage` into the block whenever it is available
- do not spend host-local tokens on mutation, review, or broad repository inspection when a matching Foreman specialist path exists
- do not perform broad host-local workspace inspection just to answer a `$cap` request unless Foreman has already degraded visibly
- if status shows `next_step=await_fan_in` with `can_advance=true` or `run_truth_surface.resume_action=advance`, treat that as a resumable Foreman boundary and call the bounded advance path; do not classify it as a degraded stall
- treat companion MCPs such as filesystem, git, fetch, docs, and OpenAI references as subordinate tools under Foreman ownership rather than direct replacements for a selected specialist
- do not treat unrelated MCP servers, OpenAI documentation surfaces, SDK helpers, or generic Codex-native helper agents as substitutes for a selected Foreman specialist role when the packaged Foreman specialist roster is available
- do not repeat unchanged visibility lines on every poll, except that `Tokens:` should still be repeated on each public captain checklist block when `token_usage` is available

## Required workflow

1. If no meaningful request remains after removing `$cap`, ask what should enter Foreman.
2. For a fresh request, call `mcp__codex_foreman__foreman_auto_entry` first.
3. If auto-entry returns a run, read `mcp__codex_foreman__foreman_status` before deciding the next move.
4. If auto-entry does not create a usable run, use an explicit Foreman plan/run entry that preserves captain planning and checklist creation. Do not fall back to `foreman_recommend_entry -> foreman_start -> foreman_orchestrate(drain_until_boundary)` as the default public path.
5. Treat the host Codex session as `captain` for the whole `$cap` request.
6. Treat the captain checklist loop as canonical:
   - captain plans
   - one specialist runs
   - captain folds the result back in
   - captain chooses the next specialist
7. Run at most one bounded specialist step at a time for the ordinary serial loop.
8. After each specialist result, return to a captain checkpoint before materializing the next specialist.
9. Do not let the host session bounce directly from `agent -> next agent` without a captain checklist update.
10. Do not default to `progression_mode=drain_until_boundary` for ordinary `$cap` work. Use bounded single-step progression so captain can inspect the result and update the checklist between specialist passes.
11. Only continue another bounded Foreman step when the captain checkpoint says more work is required and the next selected specialist is clear.
12. Reply to the operator only when the request is complete, explicitly blocked, or waiting on a real operator decision.

## Specialist selection

Use the packaged Foreman specialist roster deliberately.

- `foreman_tactician`: planning when the next bounded move is still ambiguous
- `foreman_scout`: read-only evidence gathering, repository inspection, or diagnosis
- `foreman_raider`: bounded code or config mutation
- `foreman_scribe`: bounded docs, README, release-note, or operator-guidance writing
- `foreman_arbiter`: optional review or pass/needs_work/blocked judgment when risk or ambiguity justifies it
- `foreman_sentinel`: ownership or execution-path classification only

do not route to generic Codex helper agents when a matching packaged Foreman specialist is available.

do not satisfy that role by routing to generic Codex `explorer` / `worker` agents or to unrelated MCP servers instead.

## Review policy

`arbiter` is optional, not a mandatory fixed stage.

- let `captain` close straightforward bounded work directly when the checklist acceptance is clearly satisfied
- add `arbiter` only when risk, ambiguity, failed tests, or release-critical judgment justifies a verifier pass
- if review finds `needs_work`, return to captain, update the checklist, and choose the next bounded repair agent

## Repair policy

Be conservative.

- if acceptance is not met, captain should mark the checklist item `needs_work`
- the next action should be another bounded specialist pass, not an unbounded chain drain
- keep repair loops bounded
- if bounded retry is exhausted, surface a manual boundary or a visible degraded truth instead of pretending the request completed

## Notes

- `$cap` depends on the installed `codex-foreman` MCP and the packaged local skill directory
- packaged skill updates require a fresh Codex session before the new behavior is visible
- the packaged run session can still be cleared explicitly with `$cap close current run` or `$cap clear run session`
- `$cap` remains the only public Foreman harness entrypoint; the internal specialist roster is not a public command surface
- a packaged `foreman_captain` definition may still exist for compatibility, but it is not the default public `$cap` receiver
