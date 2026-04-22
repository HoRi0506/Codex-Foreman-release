# Install Codex-Foreman 0.0.1

Use this guide for the Rust-only `0.0.1` release bundle.

## Install

1. download and unpack the release bundle for your platform
2. from the unpacked directory, run:

```bash
./bin/codex-foreman setup
./bin/codex-foreman check-install
```

3. restart Codex CLI

## Healthy Check

`./bin/codex-foreman check-install` should report:

```text
Foreman install check: status=ok version=0.0.1 entry=$cap registration=matching_registration config=present skill=matching_install
```

## What Setup Does

- registers or refreshes the `codex-foreman` MCP entry in Codex CLI
- creates or reuses `~/.config/foreman/foreman-config.json`
- installs or refreshes the public `$cap` skill under `CODEX_HOME`

## Use

After restarting Codex CLI:

```text
$cap inspect this repository and report findings only
$cap implement the scoped fix, run tests, then commit and push
```
