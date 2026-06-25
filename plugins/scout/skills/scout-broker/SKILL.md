---
name: "MCP"
description: Use Scout MCP tools for agent discovery, direct messages, project-routed asks, exact session continuity, and work item updates. Trigger this when the user wants to contact another Scout agent, delegate work, inspect who is online, or check Scout routing state from Codex.
metadata:
  short-description: Use Codex tools for Scout messaging and coordination
---

# Scout MCP

Use this plugin when the task is about Scout coordination through Codex MCP tools rather than local shell delegation.

Fast path:

1. If you know the repo/worktree, pass `currentDirectory`.
2. For tell-style writes, call `messages_send` with explicit target fields.
3. For owned work, review, investigation, or requested replies, call `ask`.
4. For durable progress, waiting, review, done, or cancellation on an existing work item, call `work_update`.
5. Use `whoami`, `agents_search`, or `agents_resolve` only when sender context is unclear, a specific target is ambiguous, or the broker says the route failed.

## Working Directory

- Pass `currentDirectory` whenever you know the relevant repo or worktree path.
- If you do not pass `currentDirectory`, Scout falls back to the plugin's default setup root.
- This plugin wrapper prefers Codex/workspace environment variables and `PWD`; `$HOME` is only the last fallback.

## Tell Vs Ask

Use `messages_send` when the user is notifying, updating, replying, or sending a one-way heads-up.
When there is one intended recipient and you know a handle such as `@hudson` or `@lattices#codex?5.5`, prefer a single `messages_send` call with `targetLabel` over a separate resolve round trip.

Use `ask` when the user wants another agent to investigate, review, decide, build, compare, or report back.

- For fresh capability work, prefer `projectPath` plus optional `harness`; Scout resolves or creates the concrete worker.
- Use `to` only when a specific agent/card/label is known and intended.
- Use `targetSessionId` only to continue one exact existing session/context.
- Set `replyMode: "inline"` only when the parent task is blocked on the answer now.
- Prefer `replyMode: "notify"` or `"none"` for background work, then use returned handles such as `flightId`, `conversationId`, `workId`, `ref`, or `targetSessionId` for follow-up.
- Use `invocations_get` to check current flight state without blocking; use `invocations_wait` only for a short bounded wait.

## Work Items

If `ask` returns a `workId` or work item, treat it as the durable handle for that delegated work.

Use `work_update` for material transitions:

- progress or claim: state `working` plus a concise `progress.summary`
- waiting: state `waiting`, `waitingOn`, and `nextMoveOwnerId`
- review: state `review` and the reviewer as `nextMoveOwnerId`
- completion: state `done` with the result summary
- cancellation: state `cancelled` with the reason

Do not bury durable progress in a second ad hoc message when a work handle exists.

## Targeting Rules

- One explicit target means a DM.
- Group coordination requires an explicit `channel`.
- Shared broadcast is opt-in; do not use it for ordinary delegation.
- Prefer project/capability routing over guessing generic handles like `claude.main`.
- Use returned handles for continuity before promoting a worker to a memorable name.
- Target shorthand is accepted where supported: `#<harness>` maps to a harness qualifier and `?<model>` maps to a model qualifier, e.g. `@lattices#codex?5.5`.
- If the user names multiple agents, fan out one action per target unless they explicitly want a shared channel update.

## Practical Defaults

- Start with a direct tool call when the user gave the route and the relevant workspace is known.
- Start with `agents_search` when the user asks who is online, available, or routable.
- Start with `agents_resolve` when the user names a specific handle and the direct write path reports ambiguity.
- Use `messages_send` for "tell", "notify", "let them know", or status updates.
- Use `ask` for "ask", "review", "investigate", "check", "build", "compare", or anything that clearly expects owned work or a reply.
- Use `work_update` when the current work item changes state.
