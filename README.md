# claude

A Pointman extension that puts Claude Code, Anthropic's coding agent, on your machines, so agents spawned as
`claude` run on it. It's a client: nothing runs beside it. The extension is only the name of npm's package,
`@anthropic-ai/claude-code`, and each node installs it.

```
pointman add claude
```

## How it works

- **Installing:** `pointman add claude` installs npm's newest Claude Code on every node of the workspace that
  hasn't got it, and on a node that joins later. `--nodes <machine>` adds it there only. It goes in the node's
  own folder, never npm's global one, so it needs no root and never clashes with a Homebrew install. The
  machine needs npm (Node.js).
- **Yours stays yours:** a Claude Code you installed yourself (Claude's own installer, Homebrew, npm, nix)
  counts as installed and comes first. `pointman add` leaves it as it is, `pointman upgrade` doesn't touch it,
  and `pointman remove` never takes it away.
- **Signing in:** sign Claude Code in once on each machine (`claude auth login`). `pointman health` and the
  apps' Machines page say when one is signed out.
- **Spawning:** a node runs agents on any client it has, so with Claude Code there, `/spawn <name> claude
  [machine] [~/folder]` in a thread starts an agent on it. Spawning on a machine without it is refused at once,
  with the fix: `pointman add claude --nodes <machine>`.

## Upgrade, remove, versions

- `pointman upgrade` moves the copies `pointman add` installed to npm's newest.
- `pointman remove claude` takes the node's own copy away, and nothing else.
- `pointman list` shows Claude Code on each machine, `added` (by `pointman add`) or `yours`, and
  `pointman health` its version, the newest there is, and whether it's signed in.
- It takes no version: a node always installs npm's newest. This extension's own version only marks changes
  to its manifest.

## Commands and settings

None. Claude Code keeps its own settings, as it does outside Pointman.

## The manifest

```toml
[client]
npm = "@anthropic-ai/claude-code"
```

That's all a client is: npm's package. A client has no `[run]`, behaviour, files, links, rules or settings.
Each node installs from its own table of packages, which has to agree with this one.

## Releasing

Bump `version` in extension.toml, commit, and make a GitHub release (not a draft or prerelease) tagged
`v<version>`. There's nothing to build, so it has no assets. The marketplace lists it within 30 minutes.

## Licence

MIT. See `LICENSE`.
