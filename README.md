# codex-foreman-release

Install-only release surface for the Rust-only `codex-foreman` `0.0.1` baseline.

This repository is not the development source. It exists to publish the release bundle, install docs, and the public `$cap` asset that the Rust binary installs into Codex.

## Layout

- `bin/codex-foreman`: Rust CLI and MCP entrypoint
- `share/skills/cap/SKILL.md`: packaged public `$cap` skill
- `docs/install.md`: install and verification guide
- `docs/release/notes/v0.0.1.md`: public release note
- `release-repo-manifest.json`: packaged release surface summary

## Install

Unpack the `0.0.1` release bundle, then run:

```bash
./bin/codex-foreman setup
./bin/codex-foreman check-install
```

Restart Codex CLI after setup.

## Notes

- npm is no longer the public install path for this baseline.
- The release bundle is expected to keep the binary and `share/skills/cap/SKILL.md` together.
- The source repository is `codex-foreman`; this repository is only the release/install surface.
