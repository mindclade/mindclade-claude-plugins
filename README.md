# mindclade-claude-plugins

A Claude Code marketplace of **32 plugins**, curated against
*Mindclade Authoritative Protocol, SDK Foundry, and Product Monorepo Production Plan v3.1*.

The plan's stack line (Part II, Global implementation constraints) is the selection authority:

> Protobuf, Buf, Nix, Go, Python, Rust, TypeScript, Protobuf-ES, Connect, gRPC, Prost, Tonic,
> PostgreSQL/Cloud SQL, Google Pub/Sub, GCS, Cloud Run, Cloud Run Jobs, Artifact Registry, KMS,
> Secret Manager, OpenTelemetry, Terraform, GitHub Actions, GitHub Apps, Next.js, pnpm.

Every entry points at its upstream repository — this repo vendors no plugin code, only the manifest.

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

- **`superpowers`** — Superpowers teaches Claude brainstorming, subagent driven development with built in code review, syste...
- **`code-review`** — Automated code review for pull requests using multiple specialized agents with confidence-based scorin...
- **`feature-dev`** — Comprehensive feature development workflow with specialized agents for codebase exploration, architect...
- **`claude-md-management`** — Tools to maintain and improve CLAUDE.md files - audit quality, capture session learnings, and keep pro...
- **`skill-creator`** — Create new skills, improve existing skills, and measure skill performance. Use when users want to crea...

### GitHub and CI

Plan stack: GitHub Actions, GitHub Apps, bot-authored release PRs, pinned third-party actions.

- **`github`** — Official GitHub MCP server for repository management. Create issues, manage pull requests, review code...
- **`git-pr-workflows`** — Git workflow automation, pull request enhancement, and team onboarding processes
- **`cicd-automation`** — CI/CD pipeline configuration, GitHub Actions/GitLab CI workflow setup, and automated deployment pipeli...

### Legacy GitLab estate

For mindclade-gitlab-estate, which still has .gitlab-ci.yml. The plan does not mention GitLab — drop these once the estate is retired.

- **`glab`** — 
- **`mr-review`** — 

### Language servers

One per SDK language: Go (reference facade), Python, Rust, TypeScript.

- **`gopls-lsp`** — Go language server for code intelligence and refactoring
- **`pyright-lsp`** — Python language server (Pyright) for type checking and code intelligence
- **`rust-analyzer-lsp`** — Rust language server for code intelligence and analysis
- **`typescript-lsp`** — TypeScript/JavaScript language server for enhanced code intelligence

### Language toolchain

uv/ruff/ty for Python; Next.js for the read-only Studio v1 (Task 39b).

- **`astral`** — Skills for working with Python using Astral tools.
- **`nextjs`** — Official Next.js skills: adopt and optimize Cache Components, adopt Partial Prefetching, and verify ru...

### Contracts and API surface

The centre of the plan: Buf/Protobuf contracts, the IR-to-OpenAPI projector, ADRs, and release declarations.

- **`protobuf`** — 
- **`documentation-generation`** — OpenAPI specification generation, Mermaid diagram creation, tutorial writing, API reference documentation
- **`api-testing-observability`** — API testing automation, request mocking, OpenAPI documentation generation, observability setup, and mo...

### Data plane

PostgreSQL authority and the migration registry, transactional outbox/inbox, GCS artifact store.

- **`cloud-sql-postgresql`** — Create, connect, and interact with a Cloud SQL for PostgreSQL database and data.
- **`database-migrations`** — Database migration automation, observability, and cross-database migration strategies
- **`google-cloud-storage`** — Official Google Cloud Storage (GCS) plugin. Manage buckets and objects, transfer data, and configure M...

### GCP platform

Cloud Run, Cloud Run Jobs, Pub/Sub, Artifact Registry, KMS, Secret Manager, Terraform.

- **`google-cloud-developer`** — Core Google Cloud guidance for coding agents: first-project onboarding, authentication and credential...
- **`terraform`** — Terraform skills for configuration, modules, providers, tests, imports, stacks, and policy.

### Training vertical

First GA implements the training vertical only: control plane, workers, attempts, checkpoints.

- **`machine-learning-ops`** — ML model training pipelines, hyperparameter tuning, model deployment automation, experiment tracking,...
- **`llm-finetuning`** — Eval-gated LLM fine-tuning lifecycle (LoRA/QLoRA, DPO, GRPO/RLVR, vision SFT, quantized export). Use w...
- **`huggingface-skills`** — Build, train, evaluate, and use open source AI models, datasets, and spaces.

### SDK and agent development

Agent SDK apps, the Claude and OpenAI APIs, HF CLI, and multi-agent workflows.

- **`agent-sdk-dev`** — Development kit for working with the Claude Agent SDK
- **`claude-api`** — Claude API and SDK documentation skill for building LLM-powered applications
- **`openai-developers`** — Build with OpenAI APIs, Agents SDK, and ChatGPT Apps from Claude Code.
- **`hf-cli`** — Execute Hugging Face Hub operations using the hf CLI. Install additional Hugging Face skills, discover...
- **`agent-teams`** — Orchestrate multi-agent teams for parallel code review, hypothesis-driven debugging, and coordinated f...

### Security and observability

Supply-chain hardening, leakage gates, OpenTelemetry traces and SLOs.

- **`security-guidance`** — Security review for Claude-generated code. Pattern-based warnings on edits, LLM-powered diff review on...
- **`security-scanning`** — SAST analysis, dependency vulnerability scanning, OWASP Top 10 compliance, container security scanning...
- **`application-performance`** — Application profiling, performance optimization, and observability for frontend and backend systems

### Architecture

C4 documentation alongside the ADR register.

- **`c4-architecture`** — Comprehensive C4 architecture documentation workflow with bottom-up code analysis, component synthesis...

## Duplicate removal

Every skill, agent, and command across the set was scanned for name collisions; the set is now
collision-free (0 duplicate skills, 0 agents, 0 commands). Removed:

| Removed | Reason |
|---|---|
| `hf-cli` | Strict subset of `huggingface-skills`, which ships `hf-cli` as one of its 25 skills |
| `skill-creator` | `claude-api` ships the identical `skill-creator` skill |
| `api-testing-observability` | Its only agent, `api-documenter`, is duplicated by `documentation-generation`, which also has `openapi-spec-generation` |
| `git-pr-workflows` | Its only agent, `code-reviewer`, is duplicated by `feature-dev`; its commands overlap the `github` MCP and `code-review` |

Known redundancy left in deliberately:

- **`claude-api` carries 16 skills unrelated to this work** (`pptx`, `xlsx`, `docx`, `pdf`,
  `algorithmic-art`, `brand-guidelines`, `canvas-design`, `slack-gif-creator`, `theme-factory`,
  and others). It is kept for its `claude-api`, `mcp-builder`, and `skill-creator` skills. Drop it
  if that context cost outweighs those three.
- **`huggingface-skills` and `llm-finetuning` overlap functionally** on TRL and LoRA training, but
  share no skill names — one is HF-ecosystem specific, the other framework-agnostic.

## Selection notes

Counts below are occurrences in the v3.1 plan document.

- **GitHub, not GitLab.** `gitlab` appears **0** times in the plan; the topology is
  `mindclade/mindclade` (private) and `mindclade/sdk` (public mirror) on GitHub Actions. The
  earlier GitLab-heavy selection was cut to `glab` and `mr-review`, kept only for the existing
  `mindclade-gitlab-estate` checkout.
- **Cloud Run, not Kubernetes.** `Kubernetes` appears **0** times, so `kubernetes-operations`
  was dropped despite the ArgoCD and kustomize trees in the `gitops/` estate repo. Re-add it if
  that platform survives the migration.
- **OpenAPI (207) and ADR (81)** are the two densest themes, which is why
  `documentation-generation` and `api-testing-observability` are in over more general
  documentation plugins.
- **Pub/Sub (59), PostgreSQL (47), GCS (24), Cloud SQL (15), Ray (35).**
- **LangChain/LangGraph/LangSmith excluded** — 2, 0, and 0 occurrences across the source repos.
- **`rust-skills` excluded** — it installs a `UserPromptSubmit` hook that injects a routing
  framework into every prompt regardless of topic. `rust-analyzer-lsp` covers the Rust SDK
  facade (Task 20) and public Rust SDK (Task 37) without the constant context cost.
- **`adr-writer` excluded** despite 81 ADR mentions — its MCP server currently fails to connect.
  `documentation-generation` provides an `architecture-decision-records` skill instead.
- **`dataproc` excluded** — configured in local settings but absent from the plan's stack.

The full 133-plugin manifest synced from this account is preserved in the first commit.

## Regenerating

```bash
claude plugin validate .
```
