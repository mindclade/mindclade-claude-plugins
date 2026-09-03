# mindclade-claude-plugins

A Claude Code marketplace of **27 plugins** curated for the Mindclade stack:
a biological AI monorepo (protein language modeling, structure prediction, and the control
plane that trains, evaluates, and serves them) on a GitLab-native estate.

Every entry points at its upstream repository — this repo vendors no plugin code, only the
manifest.

## Use it on the web

1. **Settings → Customize plugins → Add → Sync**
2. Paste `mindclade/mindclade-claude-plugins` and select **Sync**

## Use it locally

```bash
claude plugin marketplace add mindclade/mindclade-claude-plugins
```

## What's included, and why

### Process core

Planning, review, and skill authoring discipline.

- **`superpowers`** — Superpowers teaches Claude brainstorming, subagent driven development with built in code review, systematic...
- **`code-review`** — Automated code review for pull requests using multiple specialized agents with confidence-based scoring to...
- **`feature-dev`** — Comprehensive feature development workflow with specialized agents for codebase exploration, architecture d...
- **`claude-md-management`** — Tools to maintain and improve CLAUDE.md files - audit quality, capture session learnings, and keep project...
- **`skill-creator`** — Create new skills, improve existing skills, and measure skill performance. Use when users want to create a...

### GitLab workflow

The estate is GitLab-native, so MR and pipeline tooling beats GitHub PR tooling.

- **`gitlab`** — GitLab DevOps platform integration. Manage repositories, merge requests, CI/CD pipelines, issues, and wikis...
- **`glab`** — 
- **`mr-review`** — 
- **`mr-adversarial-review`** — 
- **`gitlab-pipeline-watch`** — 
- **`commit-messages`** — 
- **`stack-changes`** — 

### Language servers

One per language actually present in the tree.

- **`pyright-lsp`** — Python language server (Pyright) for type checking and code intelligence
- **`gopls-lsp`** — Go language server for code intelligence and refactoring
- **`typescript-lsp`** — TypeScript/JavaScript language server for enhanced code intelligence
- **`rust-analyzer-lsp`** — Rust language server for code intelligence and analysis

### Python toolchain

uv, ruff, and ty — matching the uv.lock / pyproject.toml setup.

- **`astral`** — Skills for working with Python using Astral tools.

### Infrastructure

OpenTofu modules, ArgoCD + kustomize environments, GCP.

- **`terraform`** — Terraform skills for configuration, modules, providers, tests, imports, stacks, and policy.
- **`kubernetes-operations`** — Kubernetes manifest generation, networking configuration, security policies, observability setup, GitOps wo...
- **`google-cloud-developer`** — Core Google Cloud guidance for coding agents: first-project onboarding, authentication and credential selec...

### ML platform

Training, evaluation, fine-tuning, and model serving.

- **`machine-learning-ops`** — ML model training pipelines, hyperparameter tuning, model deployment automation, experiment tracking, and M...
- **`llm-finetuning`** — Eval-gated LLM fine-tuning lifecycle (LoRA/QLoRA, DPO, GRPO/RLVR, vision SFT, quantized export). Use when f...
- **`huggingface-skills`** — Build, train, evaluate, and use open source AI models, datasets, and spaces.

### Contracts

buf-managed protobuf APIs in mindclade-mcdk.

- **`protobuf`** — 

### Safety and architecture

Security review and C4 architecture documentation.

- **`security-guidance`** — Security review for Claude-generated code. Pattern-based warnings on edits, LLM-powered diff review on Stop...
- **`security-scanning`** — SAST analysis, dependency vulnerability scanning, OWASP Top 10 compliance, container security scanning, and...
- **`c4-architecture`** — Comprehensive C4 architecture documentation workflow with bottom-up code analysis, component synthesis, con...

## Deliberately excluded

The full set of 133 plugins synced from this account is preserved in this repo's git history
(first commit). Notable omissions and the reason:

- **`rust-skills`** — installs a `UserPromptSubmit` hook that injects a routing framework into
  *every* prompt regardless of topic. High constant context cost for 105 Rust files.
- **`wshobson/agents` breadth** — 65 plugins from one marketplace, most covering stacks
  Mindclade does not use (mobile, Flutter, JVM, functional programming, UI design).
- **Database vendor plugins** (`alloydb`, `bigtable`, `cloud-sql-postgresql`, `dataproc`) — add
  MCP servers that boot per session; re-add individually if a workload needs one.
- **Duplicate review chains** (`comprehensive-review`, `mr-guided-review`, `git-pr-workflows`) —
  overlap `code-review` and `mr-review`.

## Regenerating

Validate after any edit:

```bash
claude plugin validate .
```
