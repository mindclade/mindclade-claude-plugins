# mindclade-plugin-policy

A Claude Code **plugin** that installs Mindclade's plugin routing policy as a skill, so Claude
picks the right plugin for a task without being told each time.

One skill: `mindclade-plugin-routing`. No agents, commands, MCP servers, or hooks — it is
guidance only, and costs nothing until it triggers.

## Install

From the marketplace:

```bash
claude plugin marketplace add mindclade/mindclade-claude-plugins
claude plugin install mindclade-plugin-policy@mindclade-claude-plugins
```

Or drop this directory into `~/.claude/skills/` to load it locally as
`mindclade-plugin-policy@skills-dir`.

## What it covers

Default posture, a task-to-plugin routing table, a do-not-route list of bundled skills that
contradict Production Plan v3.1, plan-phase mapping, an escalation ladder, release guardrails,
a token budget, and a 15-row decision record.
