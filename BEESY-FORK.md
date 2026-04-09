# Beesy Fork of RTK

This is the Beesy framework's fork of [rtk-ai/rtk](https://github.com/rtk-ai/rtk).

## What changed from upstream

- **Telemetry disabled at compile time**: `RTK_TELEMETRY_URL` is intentionally omitted
  from the build environment. The `option_env!("RTK_TELEMETRY_URL")` macro returns `None`,
  so `maybe_ping()` returns immediately without sending any data.
- **Simplified CI**: removed Discord notification and Homebrew tap update jobs.
  Build artifacts are published to GitHub Releases of this fork.
- **Release naming**: `v{upstream_version}-beesy.{n}` (e.g. `v0.35.0-beesy.1`)

## How install.sh uses this fork

`beesy-cursor-setup/install.sh` downloads the pre-compiled binary from this fork's
GitHub Releases and installs it to `~/.cursor/bin/rtk`.

URL pattern:
```
https://github.com/CRBBeesy/beesy-rtk/releases/download/v{version}-beesy.{n}/rtk-{target}.tar.gz
```

Targets:
- `aarch64-apple-darwin` (macOS Apple Silicon)
- `x86_64-apple-darwin` (macOS Intel)
- `x86_64-unknown-linux-musl` (Linux x64, static)
- `aarch64-unknown-linux-gnu` (Linux ARM64)
- `x86_64-pc-windows-msvc` (Windows x64)

## Upstream sync process (quarterly)

Every ~3 months (tracked in `beesy-cursor-setup/version.json` → `rtk.next_review`):

1. Compare this fork with upstream: `git log upstream/master..HEAD --oneline`
2. Review upstream CHANGELOG for breaking changes, new filters, security fixes
3. If worth updating: `git merge upstream/master`, test safety-guard compatibility
4. Build new release: `git tag v{new_version}-beesy.{n} && git push origin --tags`
5. Trigger `release.yml` via **Actions → Release → Run workflow** with the new tag
6. Update `beesy-cursor-setup/version.json` → `rtk` fields and `install.sh` → `RTK_VERSION`

## Adding upstream remote (for sync)

```bash
git remote add upstream https://github.com/rtk-ai/rtk.git
git fetch upstream
git log upstream/master..HEAD --oneline   # see what's new upstream
```

## Current versions

| Field | Value |
|-------|-------|
| Upstream base | v0.35.0 |
| Fork release | v0.35.0-beesy.1 |
| First release date | 2026-04-08 |
| Next review | 2026-07-08 |
