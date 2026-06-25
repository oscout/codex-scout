# Codex Scout

Codex Scout packages OpenScout's Codex integration as an installable Codex
plugin instead of requiring users to hand-enter a manual MCP server command.

The repository is named `codex-scout`. The Codex-facing plugin name is `scout`,
which keeps the installed integration short and product-level.

What it provides:

- a Codex plugin manifest so Scout can show up in the plugin catalog
- an MCP manifest that launches `scout mcp`
- a bundled Scout coordination skill so Codex knows when to search, resolve, send, ask, and update work

## Install

Add the marketplace and install the plugin from Codex:

```text
/plugin marketplace add arach/codex-scout
```

For local development:

```text
/plugin marketplace add /absolute/path/to/codex-scout
```

Launch behavior:

- prefers a locally installed `scout` CLI
- falls back to `bunx @openscout/scout`
- defaults `OPENSCOUT_SETUP_CWD` from Codex/workspace environment variables or `$PWD` when the host has not already set a Scout context root
- uses `$HOME` only as the last fallback

Advanced overrides:

- set `OPENSCOUT_SETUP_CWD` to force Scout's default workspace root
- set `OPENSCOUT_MCP_BIN` to force a specific Scout executable
