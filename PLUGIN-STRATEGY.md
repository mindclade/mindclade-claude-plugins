# Mindclade plugin use strategy

**v1.0 — final.** Decisions below are settled; each is reversible, and §8 records what was
decided and why so a later change is an amendment rather than a rediscovery.

Routing policy for the 32 plugins in `mindclade/mindclade-claude-plugins`. Paste into
`CLAUDE.md`, or reference it from there with `@PLUGIN-STRATEGY.md`.

Authority for every routing decision below is
*Mindclade Authoritative Protocol, SDK Foundry, and Product Monorepo Production Plan v3.1*.

---

## 0. What is actually loaded

Syncing this marketplace **adds a source; it does not disable anything.** As of the last check you
have **117 plugins active — 87 of them outside this curated set.** Until those are disabled in
**Settings → Customize plugins**, Claude can reach `kubernetes-operations`, `gitlab`, `rust-skills`,
`llm-application-dev`, `comprehensive-review` and 82 others that this policy deliberately excludes.

Two consequences:

- **Curation only takes effect when you disable the rest.** The 32 here are the recommendation, not
  the enforcement.
- **`glab` and `mr-review` did not sync.** Both come from `gitlab.com/gitlab-org/ai/skills.git`; if
  you need them in cloud sessions, check whether that host is reachable from the sandbox.

Where an excluded plugin overlaps one in the table below, prefer the one in the table.

### First-party plugins from the Claude directory

These five are published by Anthropic in the web plugin directory and have **no git source in
`anthropics/claude-plugins-official`**, so they cannot be aggregated into this marketplace — install
them from the directory directly.

| Plugin | Use it for | Don't use it for |
|---|---|---|
| **Bio Research** | Protein language modelling and structure-prediction literature, experiment design, research synthesis. The most relevant first-party plugin to Mindclade's actual product. | Anything in the control plane or SDK path. |
| **Engineering** | Standups, incident response. | Code review, ADRs, technical docs — `code-review`, `documentation-generation` and `c4-architecture` are the routed choices. |
| **Data** | Ad-hoc SQL, dataset exploration, dashboards for stakeholders. | Schema design or migrations — that is `database-migrations`. |
| **Design** | The Next.js Studio v1 surface only. | The rest of the monorepo, which has no design surface. |
| **Product Management** | `BLUEPRINT.md`, feature specs, roadmap framing. | Implementation planning — that is `superpowers:writing-plans`. |

---

## 1. Default posture

Before any non-trivial task, in this order:

1. **Process skill first.** `superpowers:brainstorming` for "let's build X", `superpowers:systematic-debugging`
   for "fix this bug", `superpowers:writing-plans` before multi-step work. The process skill sets the
   approach; domain plugins carry it out.
2. **Then the domain plugin** from the routing table in §2.
3. **Then verify.** `superpowers:verification-before-completion` before reporting done.

Do not skip step 1 because a task looks small. Do not load a domain plugin's skill "to see what it says" —
route from the table.

## 2. Routing table

| When the task is about | Use |
|---|---|
| Protobuf, Buf, gRPC, Connect, contract changes under `contracts/` or `protocols/` | `protobuf` |
| OpenAPI projection, public descriptor, IR-to-OpenAPI, spec diffing | `documentation-generation:openapi-spec-generation` |
| Writing or amending an ADR | `documentation-generation:architecture-decision-records` |
| Release notes, changelogs, release declarations | `documentation-generation:changelog-automation` |
| System/architecture documentation, boundary diagrams | `c4-architecture` agents, `documentation-generation:mermaid-expert` |
| PostgreSQL schema, migration registry, outbox/inbox tables | `database-migrations` (`/sql-migrations`) |
| Cloud SQL instance health, replication, config, lifecycle | `cloud-sql-postgresql` |
| GCS buckets, artifact transfer, object lifecycle, bucket security | `google-cloud-storage` |
| Pub/Sub, Cloud Run, Cloud Run Jobs, Artifact Registry, KMS, Secret Manager, `gcloud` | `google-cloud-developer` |
| Terraform modules, providers, state, policy, module tests | `terraform` |
| GitHub Actions workflows, pinning, secrets, deployment pipelines | `cicd-automation` |
| PRs, issues, repo operations on `mindclade/mindclade` or `mindclade/sdk` | `github` MCP |
| Reviewing a diff or branch | `/code-review` |
| Building a feature end to end | `/feature-dev` (`code-architect` → `code-explorer` → `code-reviewer`) |
| Python: uv locks, ruff, ty | `astral` |
| Go / Python / Rust / TypeScript navigation, refs, rename | the matching `*-lsp` plugin (passive) |
| Next.js Studio v1 work | `nextjs` |
| Training control plane, ML pipelines, checkpoints, MLOps | `machine-learning-ops` |
| Fine-tuning method choice, eval harness, LoRA/GRPO, checkpoint promotion | `llm-finetuning` |
| HF datasets, transformers, TRL, model hub, local models | `huggingface-skills` |
| Building an Agent SDK app, verifying one | `agent-sdk-dev` |
| Claude API usage, model ids, pricing, caching, MCP server authoring | `claude-api:claude-api`, `claude-api:mcp-builder` |
| OpenAI API usage in the codebase | `openai-developers:openai-docs`, `:openai-api-troubleshooting` |
| Threat modelling, STRIDE, SAST config, attack trees, supply-chain review | `security-scanning` |
| Latency, tracing, OpenTelemetry, SLOs, observability | `application-performance` |
| Parallel work across independent files, multi-dimension review | `agent-teams` |
| Keeping `CLAUDE.md` accurate | `claude-md-management` |
| Legacy `mindclade-gitlab-estate` only | `glab`, `mr-review` |

## 3. Do not route here

These ship inside kept plugins but are wrong for Mindclade. Never load them; if one seems to fit,
the task has been misread.

| Skill or agent | Why not |
|---|---|
| `cicd-automation:gitlab-ci-patterns` | The plan is GitHub Actions. GitLab appears 0 times in it. |
| `cicd-automation:kubernetes-architect` | The plan is Cloud Run and Cloud Run Jobs. Kubernetes appears 0 times. |
| `terraform:azure-verified-modules` | GCP shop. No Azure. |
| `huggingface-skills:hf-cloud-*` (6 skills) | AWS SageMaker. Wrong cloud — use `google-cloud-developer`. |
| `machine-learning-ops:recsys-pipeline-architect` | Not a recommender-systems product. |
| `openai-developers:build-chatgpt-app`, `:chatgpt-app-submission` | Not building ChatGPT apps. |
| `base44` (whole plugin) | Hosted app-builder with its own CLI, SDK, and deploy path. The plan commits to Buf contracts, hand-written SDK façades, an IR-first OpenAPI projector, and Cloud Run via Terraform. No seam for it, and §5.3 prohibits the dependency. Revisit only as a plan amendment with an ADR. |
| `claude-api` office and design skills (`pptx`, `xlsx`, `docx`, `pdf`, `algorithmic-art`, `brand-guidelines`, `canvas-design`, `slack-gif-creator`, `theme-factory`, `web-artifacts-builder`, `internal-comms`, `academy-guide`, `discernment-nudge`) | Carried only because the plugin is monolithic. 16 of its 19 skills are dead weight. |

The `gitops/` estate repo still uses ArgoCD, kustomize, and the GPU operator. That predates the plan.
If asked to work there, say the plan does not cover it and ask whether the platform survives migration
before reaching for Kubernetes tooling that is not installed.

## 4. Plan-phase mapping

| Phase | Primary plugins |
|---|---|
| 0 — source authority, monorepo governance | `documentation-generation` (ADRs), `github`, `cicd-automation` |
| 1 — hermetic contracts, boundaries | `protobuf`, `documentation-generation`, `code-review` |
| 2 — PostgreSQL, eventing, artifacts, training vertical | `database-migrations`, `cloud-sql-postgresql`, `google-cloud-storage`, `google-cloud-developer`, `machine-learning-ops` |
| 3 — internal SDK façades, protocol v1 | `gopls-lsp`, `pyright-lsp`, `rust-analyzer-lsp`, `typescript-lsp`, `astral`, `feature-dev` |
| 4 — public boundary, OpenAPI projection | `documentation-generation:openapi-spec-generation`, `security-scanning` (leakage gate) |
| 5 — Foundry, public SDKs, mirror promotion | `agent-sdk-dev`, `claude-api:mcp-builder`, `code-review`, `documentation-generation` |
| 6 — hosted ops, supply chain, recovery, GA | `security-scanning`, `application-performance`, `terraform`, `google-cloud-developer`, `nextjs` |

## 5. Escalation ladder

Stay at the lowest rung that works.

1. **One skill** — the default. Most tasks end here.
2. **One agent** — when the task needs its own context window (deep search, a focused review).
3. **`agent-teams`** — only for genuinely parallel work with clean file-ownership boundaries.
   `/team-review` for multi-dimension review, `/team-feature` for parallel implementation.
4. **Workflows** — only on explicit request. Never infer one.

Do not spawn a team for work one agent can do. Do not spawn an agent for work one skill can do.

## 6. Guardrails

- **Never** push to `mindclade/sdk`, cut a release tag, or promote to the public mirror without explicit
  per-action approval. §12 of the plan makes these irreversible.
- **Never** run a migration against a live Cloud SQL instance without approval, even in staging.
- Treat the public leakage gate (§13.6) as blocking: if a change touches the sanitized public bundle,
  stop and say so rather than working around it.
- `github` MCP auth is currently broken (`Authorization header is badly formatted`). If a GitHub
  operation fails, report it — do not fall back to shelling out with unreviewed credentials.

---

## 7. Token budget

Plugin count is the wrong lever. Measured always-on context, from the local plugin catalog
(28 of 117 plugins priced, so both figures are floors):

| Set | Always-on | Heaviest member |
|---|---|---|
| Live, 117 plugins | ≥ 18,793 | `data-engineering` — 6,060 |
| This policy, 32 plugins | ≥ 8,736 | `huggingface-skills` — 5,111 |

`huggingface-skills` is 58% of this set's measured cost on its own. `terraform`, `github`,
`security-guidance` and all four language servers are **0**.

**Rule:** before adding a plugin, check its `always_on` figure with `claude plugin details <name>`.
Anything above ~1,000 needs a reason beyond "might be useful".

## 8. Decision record

| # | Decision | Rationale | Reversal cost |
|---|---|---|---|
| 1 | 32 plugins, not 133 | 133 was everything installed locally, most of it for stacks Mindclade does not use | Low — full manifest is in the repo's first commit |
| 2 | GitHub tooling over GitLab | GitLab appears 0 times in plan v3.1; topology is `mindclade/mindclade` + `mindclade/sdk` on GitHub Actions | Low — re-add `gitlab`, restore the 5 cut GitLab plugins |
| 3 | No `kubernetes-operations` | Kubernetes appears 0 times; the plan deploys to Cloud Run and Cloud Run Jobs | Low, but revisit if the `gitops/` ArgoCD platform survives migration |
| 4 | Keep `huggingface-skills` despite 5,111 always-on | Protein language modelling is the product; HF is that ecosystem | Free — dropping it takes the set to ~3,600 always-on |
| 5 | Keep `claude-api` despite 16 irrelevant skills | Carried for `claude-api`, `mcp-builder`, `skill-creator`; the plugin is monolithic and cannot be split | Free — drop it and lose those three |
| 6 | Exclude `base44` | Contradicts the contract-first, self-hosted SDK architecture | Requires an ADR, not a plugin change |
| 7 | Exclude `adr-writer` despite 81 ADR mentions | Its MCP server fails to connect; `documentation-generation` covers ADRs | Low — re-add once auth works |
| 8 | Exclude `rust-skills` | `UserPromptSubmit` hook injects a routing framework into every prompt regardless of topic | Low, but the constant cost returns |

## 9. Appendix — plugins to disable

The 87 currently live and outside this set. Disable in **Settings → Customize plugins**; the
curation has no effect until this is done.

**Contradicts the plan** (16)

`kubernetes-operations` `gitlab` `cloud-infrastructure` `infra-ci-cd-turborepo-ci` `shared-monorepo-turborepo` `shared-monorepo-pnpm-workspaces` `alloydb` `alloydb-omni` `bigtable` `dataproc` `knowledge-catalog` `dgx-spark-ops` `nixos-managing` `cloudflare` `ckeditor` `meigen-ai-design`

**Overlaps a routed plugin** (41)

`comprehensive-review` `code-documentation` `code-refactoring` `codebase-cleanup` `performance-testing-review` `error-debugging` `debugging-toolkit` `database-design` `database-cloud-optimization` `api-testing-observability` `api-scaffolding` `backend-development` `backend-api-security` `git-pr-workflows` `tdd-workflows` `testing-tdd` `unit-testing` `full-stack-orchestration` `deployment-strategies` `deployment-validation` `dependency-management` `framework-migration` `context-management` `agent-orchestration` `skill-creator` `code-simplifier` `data-validation-suite` `documentation-standards` `team-collaboration` `developer-essentials` `conductor` `operating-kit` `ship-mate` `compound-engineering` `superself` `skill-forge-essentials` `before-you-build` `claude-code-setup` `plugin-eval` `adr-writer` `avoid-ai-writing`

**Wrong stack or domain** (29)

`accessibility-compliance` `ui-design` `frontend-design` `frontend-react` `frontend-mobile-development` `multi-platform-apps` `javascript-typescript` `typescript-patterns` `jvm-languages` `functional-programming` `systems-programming` `python-development` `shell-scripting` `api-batch-processor` `ai-ml-engineering-pack` `anthropic-pack` `data-engineering` `llm-application-dev` `langchain-mcp` `langchain-skills` `langsmith-mcp` `langsmith-skills` `sentry-skills` `playwright` `hol-guard` `protect-mcp` `review-agent-governance` `signed-audit-trails` `block-no-verify`

**High always-on cost for the value** (1)

`rust-skills`

