# Codex Things

Personal Codex skills and helper scripts packaged as a repo-local plugin marketplace.

## Contents

- `personal-codex-tools`: a plugin containing:
  - `kde-computer-use`: KDE Plasma desktop inspection and automation helpers.
  - `practical-code-optimization`: profiling-first optimization guidance and baseline scripts.

## Layout

```text
.agents/plugins/marketplace.json
plugins/personal-codex-tools/.codex-plugin/plugin.json
plugins/personal-codex-tools/skills/
```

The marketplace entry points at `./plugins/personal-codex-tools`, so keep the `plugins/` directory next to `.agents/`.

## Install

From GitHub:

```bash
codex plugin marketplace add https://github.com/arm64be/codex-things.git
codex plugin add personal-codex-tools@codex-things
```

For local development from a clone of this repo:

```bash
codex plugin marketplace add .
codex plugin add personal-codex-tools@codex-things
```

You can confirm Codex sees the plugin with:

```bash
codex plugin list --marketplace codex-things
```

The marketplace manifest lives at `.agents/plugins/marketplace.json`, and its marketplace name is `codex-things`.

The bundled scripts are stored inside their skills so an installed plugin carries the instructions and the deterministic helpers together.
