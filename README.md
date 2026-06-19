# renovate-config

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
| `default.json` | `github>hasansezertasan/renovate-config` | Base config: `config:recommended`, semantic commits, dependency dashboard, `internal` label. |

## Notes

- The Mend Renovate GitHub App must have access to **both** this repo and any repo
  that extends it (automatic if this repo is public).
- This is Renovate's [config presets](https://docs.renovatebot.com/config-presets/)
  mechanism — distinct from GitHub's `.github` community-health-files repo, which
  Renovate does not read.
