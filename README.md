# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) configuration presets for the `bacluc-agent` organization: a regex `customManager` that picks up versions written as `# renovate: …` markers above `KEY=value` lines.

## Use it from another repository

Add a `renovate.json` to the consuming repository:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>bacluc-agent/renovate-config"]
}
```

This repository's `default.json` is also the [organization-level onboarding default](https://docs.renovatebot.com/config-presets#grouporganization-level-presets), so new repositories in the org get it automatically.

## The marker format

Put the marker comment directly above the version assignment:

```bash
# renovate: datasource=docker depName=ghcr.io/bacluc/prettier-image/prettier-image
PRETTIER_VERSION=5.1.0

# renovate: datasource=docker depName=rhysd/actionlint
ACTIONLINT_VERSION=1.7.12
```

## Matching constraints

- The exact comment `# renovate: datasource=<datasource> depName=<depName>` must be on the line directly above the assignment; only whitespace may intervene.
- The key must be `[A-Z_]+` immediately followed by `=` or `:` — no spaces around the separator, so `ALPINE_VERSION = 1.2.3` does **not** match.
- The value must start with a digit and must not carry a `v` prefix: `VERSION=v1.2.3` is ignored.
- Quoted values are allowed (`PRETTIER_VERSION: "5.1.0"`).
- `ARG ` and `ENV ` prefixes do **not** match (`ARG ALPINE_VERSION=3.24.2`, `ENV X=1.2.3` are ignored) — use a repository-local `customManagers` entry instead (see https://github.com/bacluc-agent/ask-ai/blob/main/renovate.json).
- LF line endings only.

## Enabling it

`custom.regex` runs by default, so no `enabledManagers` entry is needed unless you set `enabledManagers` yourself.
