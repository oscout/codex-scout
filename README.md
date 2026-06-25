# Codex Scout

This repository contains the Codex plugin marketplace for OpenScout's Codex
integration. The repository is named `codex-scout`; the Codex-facing plugin is
named `scout` so Scout appears as the short, product-level integration inside
Codex.

Website: <https://arach.github.io/codex-scout/>

Repository: <https://github.com/arach/codex-scout>

## Included Plugins

- `scout`: a Codex plugin that launches the Scout MCP server and includes a
  Scout coordination skill for agent discovery, direct messages, asks, session
  continuity, and work updates.

## Install Locally

From Codex, add this repository as a local plugin marketplace:

For local development, point Codex at this checkout:

```text
/plugin marketplace add /Users/arach/dev/codex-scout
```

Codex records marketplaces in `~/.codex/config.toml`. If the plugin does not
appear in your next session after adding the marketplace, enable it explicitly:

```toml
[plugins."scout@openscout"]
enabled = true
```

## Install From GitHub

```text
/plugin marketplace add arach/codex-scout
```

The marketplace declares the `scout` plugin as installed by default. If your
Codex build only adds the marketplace and does not enable the plugin, add this
entry to `~/.codex/config.toml`:

```toml
[plugins."scout@openscout"]
enabled = true
```

## Plugin Behavior

The plugin starts Scout's MCP server through `scripts/run-scout-mcp.sh`.

Launch behavior:

- prefers a locally installed `scout` CLI
- falls back to `bunx @openscout/scout`
- defaults `OPENSCOUT_SETUP_CWD` from Codex/workspace environment variables or
  `$PWD` when the host has not already set a Scout context root
- uses `$HOME` only as the last fallback

Advanced overrides:

- set `OPENSCOUT_SETUP_CWD` to force Scout's default workspace root
- set `OPENSCOUT_MCP_BIN` to force a specific Scout executable

## Website

The static project page lives at [`docs/index.html`](./docs/index.html). GitHub
Pages can serve it from the `docs/` folder on `main`.

## Notes

This is an experimental local developer package. It depends on a local
OpenScout broker and a `scout` CLI, or Bun so the wrapper can run
`bunx @openscout/scout`.
