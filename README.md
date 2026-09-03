# renovate-config

[![Validate Renovate presets](https://github.com/hasansezertasan/renovate-config/actions/workflows/validate.yml/badge.svg)](https://github.com/hasansezertasan/renovate-config/actions/workflows/validate.yml)

Shared [Renovate](https://docs.renovatebot.com/) configuration presets for my repositories.

Instead of duplicating Renovate settings in every project, each repo extends the
preset(s) defined here. Change a setting once, and every consuming repo picks it
up on its next Renovate run.

## Usage

In any repo, replace the contents of `renovate.json` (or `.github/renovate.json`)
with:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>hasansezertasan/renovate-config"]
}
```

The bare `github>hasansezertasan/renovate-config` shorthand resolves to
[`default.json`](./default.json) at the root of this repo.

### Per-repo overrides

Add repo-specific settings *after* the preset — later entries win:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>hasansezertasan/renovate-config"],
  "packageRules": [
    { "matchUpdateTypes": ["minor", "patch"], "automerge": true }
  ]
}
```

### Pinning

Pin to a tag or branch for reproducibility:

```json
{
  "extends": ["github>hasansezertasan/renovate-config#v1.0.0"]
}
```

## Presets

| Preset | Reference | Description |
| ------ | --------- | ----------- |
| `default.json` | `github>hasansezertasan/renovate-config` | Base config: `config:recommended`, semantic commits, dependency dashboard, `internal` label, and a `prek.toml` hook-`rev` custom manager (Renovate's built-in pre-commit manager only reads `.pre-commit-config.yaml`). **Automerges green GitHub Actions and prek-hook `rev` updates.** Never auto-merges Python version (`.python-version`) bumps. |
| `python.json` | `github>hasansezertasan/renovate-config:python` | Extends the base, adds `uv.lock` lock-file maintenance and `pep621` grouping. **Automerges green dev/test/lint/docs dependency updates.** Never auto-merges direct (runtime) dependencies. |

> **Auto-merge is green-only and scoped:** a matching PR merges automatically only after all its checks pass. This is enforced with `platformAutomerge: false` (see below) — Renovate — not GitHub — decides when a branch is green, so optional checks can't slip a premature merge through. Direct (runtime) dependencies and Python version bumps are **never** auto-merged — the owner reviews those manually. Override any rule per-repo via `packageRules` (see *Per-repo overrides*).

### Python / `uv` projects

The `python.json` preset already extends `default.json`, so reference it alone:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>hasansezertasan/renovate-config:python"]
}
```

## Validation

Every preset is checked in CI by Renovate's official
[`renovate-config-validator`](https://docs.renovatebot.com/config-validation/)
via [`.github/workflows/validate.yml`](./.github/workflows/validate.yml) on each
push and pull request, so a broken preset can't reach `main`. Run it locally too:

```sh
npx --yes --package renovate -- renovate-config-validator --strict default.json python.json
```

## Notes

- The Mend Renovate GitHub App must have access to **both** this repo and any repo
  that extends it (automatic if this repo is public).
- This is Renovate's [config presets](https://docs.renovatebot.com/config-presets/)
  mechanism — distinct from GitHub's `.github` community-health-files repo, which
  Renovate does not read.
