# mindclade-claude-plugins

A Claude Code plugin marketplace aggregating the 133 plugins enabled in Rob's
local Claude Code install, so the same set can be synced into Claude Code on the web.

Every entry points at its upstream repository — this repo contains no vendored
plugin code, only the manifest.

## Use it on the web

1. Push this repo to GitHub (e.g. `mindclade/mindclade-claude-plugins`).
2. In Claude Code on the web: **Settings → Customize plugins → Add → Sync**.
3. Paste `mindclade/mindclade-claude-plugins` and select **Sync**.

## Use it locally

```bash
claude plugin marketplace add mindclade/mindclade-claude-plugins
```

## Regenerating

The manifest is generated from `~/.claude/settings.json` (`enabledPlugins`) plus the
cached upstream manifests in `~/.claude/plugins/marketplaces/`. Nine plugins whose
upstream marketplace lists them at the repo root use `source: url`; the rest use
`git-subdir`. Nine cross-listed duplicates were deduped, preferring
`claude-plugins-official` over vendor and aggregator marketplaces.

Validate after any edit:

```bash
claude plugin validate .
```
