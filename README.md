# claude

A Pointman extension that puts Claude Code, Anthropic's coding agent, on your machines, so agents spawned as
`claude` run on it. It's a client: nothing runs beside it.
Its manifest says how a node installs it, signs it in and talks to it (`[client]`, v2).

```
pointman add claude
```

## How it works

- **Installing:** `pointman add claude` installs Anthropic's native build of Claude Code on every node of the
  workspace that hasn't got it, and on a node that joins later. `--nodes <machine>` adds it there only. The build
  is checked against the sha-256 in Anthropic's manifest, then set up by its own `claude install`, which links
  `~/.local/bin/claude` and keeps it up to date. Nothing else is needed on the machine.
- **Yours stays yours:** a Claude Code you installed yourself (Claude's own installer, Homebrew, npm, nix)
  counts as installed and comes first. `pointman add` leaves it as it is, `pointman upgrade` doesn't touch it,
  and `pointman remove` never takes it away.
- **Signing in:** sign Claude Code in once on each machine (`claude auth login`). `pointman health` and the
  apps' Machines page say when one is signed out.
- **Spawning:** a node runs agents on any client it has, so with Claude Code there, `/spawn <name> claude
  [machine] [~/folder]` in a thread starts an agent on it. Spawning on a machine without it is refused at once,
  with the fix: `pointman add claude --nodes <machine>`.

## Upgrade, remove, versions

- `pointman upgrade` moves the copies `pointman add` installed to their newest.
- `pointman remove claude` takes the node's own copy away, and nothing else.
- `pointman list` shows Claude Code on each machine, `added` (by `pointman add`) or `yours`, and
  `pointman health` its version, the newest there is, and whether it's signed in.
- It takes no version: a node always installs the newest. This extension's own version only marks changes
  to its manifest.

## Commands and settings

None. Claude Code keeps its own settings, as it does outside Pointman.

## The manifest

```toml
[client]
npm = "@anthropic-ai/claude-code"          # where its newest version is said
binaries = ["claude"]
protocol = "claude-stream"                  # stream-json in and out
api_keys = ["ANTHROPIC_API_KEY", "ANTHROPIC_AUTH_TOKEN"]   # never passed to a run: runs are on its own login

[client.install]
from = "native"                             # its own installer (claude install latest)

[client.sign_in]
how = "claude auth login"
check = ["auth", "status", "--json"]

[client.mcp]
via = "flag"
args = ["--mcp-config", "{value}"]
```

`[client]` v2 (Pointman release 20) says what a node needs: its programs, how its runs talk (`protocol`), how a
node installs it, how it signs in, how MCP servers reach it, and its vendor's API keys, which never reach a run.
Claude Code is built into the node as code, and this manifest has to say what that code does: a test in Pointman
checks they agree. A client still stands alone: no `[run]`, behaviour, files, links, rules or settings. The
format is in Pointman's docs/dev/extensions.md.

## Releasing

Bump `version` in extension.toml, commit, and make a GitHub release (not a draft or prerelease) tagged
`v<version>`. There's nothing to build, so it has no assets. The marketplace lists it within 30 minutes.

## Licence

MIT. See `LICENSE`.
