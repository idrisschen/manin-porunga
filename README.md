<p align="center">
  <img src="docs/assets/porunga-logo-512.png" width="140" alt="Manin Porunga" />
</p>

<h1 align="center">Manin Porunga</h1>

<p align="center">
  <a href="https://github.com/idrisschen/manin-porunga/actions/workflows/evals.yml"><img src="https://github.com/idrisschen/manin-porunga/actions/workflows/evals.yml/badge.svg" alt="doctrine evals" /></a>
</p>

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/claude-ai.svg" width="30" title="Claude Code" alt="Claude" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/openai.svg" width="30" title="GPT lineage — Leo, and Codex as execution fallback" alt="OpenAI" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/deepseek.svg" width="30" title="DeepSeek — default execution engine" alt="DeepSeek" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/obsidian.svg" width="30" title="Obsidian vault — the memory" alt="Obsidian" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/github.svg" width="30" title="git — the canon" alt="GitHub" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/python.svg" width="30" title="Python — engine & evals" alt="Python" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/postgres.svg" width="30" title="PostgreSQL + pgvector — agent memory" alt="PostgreSQL" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/discord.svg" width="30" title="Discord — organ notifications" alt="Discord" />&nbsp;&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/notion.svg" width="30" title="Notion — archive of machine outputs" alt="Notion" />
</p>

<p align="center">
  <strong>A VPS-based personal AI control room — two peer agents from different model families, Claude as orchestrator, cheap execution engines, eval-tested guardrails, and gated delivery.</strong><br />
  Porunga runs on one VPS. This public repository is a curated mirror, not the deployment source.
</p>

<p align="center">
  <sub>Curated public mirror of a private system. Kept deliberately
  dense — only the pieces worth reading: the <a href="#evaluation">CI-gated eval harness</a>,
  the <a href="#notable-delivery-contract--gating">notable delivery gate</a>, the
  <a href="#showcase-semantic-vault-search">offline retrieval demo</a>, the
  operating doctrine, and the mechanical guardrails.</sub>
</p>

<p align="center">
  <sub>A worked example of context engineering, multi-agent orchestration, LLMOps, and LLM evals.
  See <a href="#what-this-demonstrates-engineering">what this demonstrates</a>.</sub>
</p>

---

## Why

The interesting part of a personal AI system is **where it runs, who decides, which model
does the work, and which source wins when copies disagree**. Porunga runs on one VPS:

- **Hublot**, a private PWA hub, is where the operator talks to the agents. A Claude Router
  answers or dispatches leased jobs; every external action goes through a human approval gate.
- **Two peer majordomos** share the host under separate identities — Jarvis (Claude lineage)
  and Leo (GPT lineage, Hermes agent) — with separate memories, so either can challenge the other.
- **Claude orchestrates, it does not execute.** Routines, delegated tasks and code sessions run on
  DeepSeek Flash, fall back to Codex on quota exhaustion, and never silently switch back to Claude.
- **Eight systemd timers** run the organs (production health, brief, watchdogs, backups, sync)
  and report to Discord. The workstation is an interactive client only.

The private Porunga repository and private knowledge base remain the sources of truth. This
repository is a sanitized public mirror with examples, guardrail scripts, and evaluation
fixtures. It is not pulled by the VPS and must not be treated as the production deployment
source. Notion is an archive for selected machine outputs, not an authority for code or doctrine.

See the [current architecture, engine routing and timer inventory](docs/architecture.md) (verified 2026-11-03).

<p align="center">
  <a href="docs/assets/setup-map.html"><img src="docs/assets/setup-map.png" alt="Porunga architecture: operator clients reach the Hublot hub on the sole VPS; a Claude Router dispatches jobs to Jarvis and Leo; execution runs on DeepSeek with Codex fallback; eight timers report to Discord; private Git and vault remain the sources of truth; this public repository is not deployed." width="100%" /></a>
</p>

## What this demonstrates (engineering)

> What each part of this system *is*, in the vocabulary of building production AI systems —
> the same competencies engineering teams evaluate for.

| Component in this repo | AI-engineering competency |
|---|---|
| Tiered **HOT / WARM / COLD** memory + **path-scoped rules** + **cross-harness bridges** ([`AGENTS.md`](./AGENTS.md)) | **Context engineering** — managing token budget deliberately instead of dumping everything in. The public mirror shows portable doctrine and examples; private runtime wiring stays private. |
| **Offline retrieval demo** ([`showcase/semantic-vault-search/`](./showcase/semantic-vault-search/)) | **Retrieval / RAG** — a runnable hybrid retrieval prototype. In production, knowledge lookup uses structured indexes and lexical search, and agent memory is recalled from PostgreSQL + pgvector with a local multilingual embedding model; the demo itself is not deployed. |
| Hublot hub, leased workers, two peer agents from different model families ([architecture](docs/architecture.md)) | **Multi-agent orchestration** — a Router dispatches jobs under leases, agents keep separate identities and memories, irreversible calls get a cross-family second opinion, and external actions require human approval. |
| Engine routing: Claude orchestrates, DeepSeek executes, Codex on quota | **LLM cost engineering** — the expensive model only frames, reviews and replies; execution is routed through quota gates with a single, logged fallback and no silent escalation. |
| **Notable Delivery Contract** ([`bin/jarvis-ship-check.py`](./bin/jarvis-ship-check.py)) | **Delivery engineering & QA** — moving beyond "it compiles": pre-code hypothesis audits, vertical observable slices with verifiable E2E proof, and independent review in fresh context before calling a feature "ready". |
| Doctrine evaluation harness ([`tests/doctrine/`](./tests/doctrine)) | **LLM evaluation** — assistant behaviour tested against 14 scenarios across 6 categories, not assumed correct: weighted scoring, category aggregation, **regression detection vs previous run**, non-zero exit gating CI. |
| VPS-only background runtime, eight observable timers, deterministic token observability ([`bin/jarvis-token-report`](./bin/jarvis-token-report), reference script) | **LLMOps** — every organ has a reader of its verdict (success, failure, absence, delay); deterministic organs use no model at all. The token report here is an example, not a live service. |
| PreToolUse hooks, sequential state ops, memory boundaries ([`bin/jarvis-bash-guard.sh`](./bin/jarvis-bash-guard.sh), [`bin/jarvis-memory-guard.sh`](./bin/jarvis-memory-guard.sh)) | **AI safety & reliability** — mechanical guardrails around an autonomous agent: intercepts batched mutating git commands and `rm -rf`, enforces headless write-roots boundaries, and warns before "dumb zone" marathon sessions. |
| Incident-forged, **dated** operating rules | **Production discipline** — real failures turned into enforced checks, not blog best-practices. |

> **Reading this as a recruiter / AI engineer?** The full competency→evidence map,
> key numbers, honest limits and anticipated interview answers are in
> **[AI Engineer signals](docs/ai-engineer-signals.md)**.

## Runtime boundary

| Layer | Location | Responsibility |
|---|---|---|
| Porunga runtime | **VPS — sole execution host** | Hublot hub and relays, both agents' workers, scheduled organs, memory databases, isolated agent browsers. |
| Operator access | Hublot PWA, private Telegram DM, Claude remote sessions, Discord | Talks to the Router, approves external actions, reads organ verdicts. |
| Workstation | Interactive client | Vault editing, computer use, IDE; hosts no Porunga timer, background organ, or persistent store. |
| Private canon | Private Git repository and knowledge base | Holds production code, configuration, doctrine, and private data. |
| This repository | Public GitHub mirror | Holds sanitized documentation, examples, and evaluation material; it is not deployed. |

The word **VPS** is the public-facing name for the runtime. The provider, host identity,
network details, and private paths are omitted from this repository.

## Features

- **Tiered memory** — HOT loads every session, WARM on context match, COLD only
  on explicit request. Admission to HOT is strict: relevant in ≥ 50 % of
  sessions or a high-blast-radius guardrail. *"The garage must not become the house."*
- **Path-scoped rules** — a project's doctrine (target infra, deploy gotchas,
  "never DELETE in prod via API") loads *because I opened that project's directory*,
  not because I said a magic word. File-path matching is mechanical and deterministic.
- **Cross-harness portability** — standard [`AGENTS.md`](./AGENTS.md) + [`bin/jarvis-codex-bridge`](./bin/jarvis-codex-bridge)
  project the same single-source-of-truth doctrine into Claude Code and Codex
  ([`bin/jarvis-antigravity-bridge`](./bin/jarvis-antigravity-bridge) kept as reference — retired from production in October 2026).
- **Notable Delivery gate** — client work runs through `/jarvis-ship` and is validated
  mechanically by [`bin/jarvis-ship-check.py`](./bin/jarvis-ship-check.py): every feature in `feature_list.json`
  must be marked `done` AND carry demonstrable `evidence`.
- **Pre-code hypothesis audit** — before writing code, document in the spec any assumptions
  affecting architecture or external actions. Don't interrogate the user on reversible choices;
  block only on irreversible forks.
- **Self-critique before "ready"** — tests green ≠ prod-ready. Client code requires a spontaneous
  risk analysis (🔴 critical / 🟡 watch / 🟢 minor) and **observable E2E tests of the real flows**.
- **Incident-forged guardrails** — dated rules with the scar attached, enforced
  mechanically where possible: `jarvis-bash-guard.sh` blocks batched mutating git commands,
  `jarvis-memory-guard.sh` limits memory bloat, and `jarvis-context-watch.sh` warns before "dumb zone" sessions.
- **Tested doctrine** — the assistant's policy rules live in `tests/doctrine/` as scenarios;
  the rules are *tested in CI*, not just written.
- **Operator hub with an approval gate** — Hublot: one thread for the operator and both agents,
  leased jobs, owner-only Telegram bridge, and no third-party action without a human-approved draft.
- **Engine routing** — Claude for orchestration only; DeepSeek Flash by default, Codex once on
  HTTP 402/429, postponed rather than escalated when every gate is closed.
- **VPS runtime** — scheduled Porunga work runs on the VPS through eight systemd user timers that
  report to Discord. The current inventory is in [`docs/architecture.md`](docs/architecture.md).
- **Curated code** — this mirror keeps standalone, runnable pieces and historical references;
  it is not a complete copy of the production runtime.

## The memory model

| Tier | When loaded | What goes there |
|---|---|---|
| **HOT** | every session (`@import` in `CLAUDE.md`, bridged to `AGENTS.md`) | persona (`SOUL`), user profile, active decisions, core operational workflows (<100 KB total) |
| **WARM** | on context match (cwd / path-scoped rules) | one file per project, domain, or infra runbook |
| **COLD** | only on explicit request | historical archives, past incident post-mortems, raw session logs |
| **path-scoped** | mechanically, when opening matching code | project-specific client/infra rules (`claude-config/rules/`) |

## Evaluation

This repo ships a real **LLM-evaluation harness** for the assistant's behavioural
**doctrine** — its *policy engine*: the safety and behaviour rules the agent must
obey. Each rule (persona, truthfulness, safety, memory discipline, delivery contract)
is a graded scenario across 6 categories; the harness computes weighted scores per category,
detects regressions vs the previous run, and exits non-zero on failure so it gates CI.

```bash
python3 tests/doctrine/runner.py                # offline, deterministic, CI-safe (~8 ms)
python3 tests/doctrine/runner.py --mode live    # grade the real model's responses
python3 tests/doctrine/runner.py --mode judge   # live + optional LLM-judge
```

**Offline deterministic baseline** — graded against recorded reference responses;
a reproducible CI number and regression guard, *not* a live-model capability score:

| Metric | Value |
|---|---|
| Scenarios | 14 across 6 categories |
| Baseline pass rate | 100% (14/14) |
| Critical scenarios | 8/8 |
| Suite latency | ~8 ms total |
| Regression vs previous run | none |

Full design: [`tests/doctrine/README.md`](tests/doctrine/README.md).

## Notable Delivery contract & Gating

AI coding assistants frequently suffer from premature declaration of victory: declaring
"all done" because a unit test passed or syntax checked. Porunga enforces a mechanical gate:

```bash
# Generate a scaled checklist template (landing / sprint / app)
python3 bin/jarvis-ship-template.py sprint "User authentication & reset" my-app

# Mechanically validate completion and evidence before shipping
python3 bin/jarvis-ship-check.py ~/.local/var/jarvis-ship/my-app-user-authentication.json
```

1. **Pre-code hypotheses**: document proof, impact if false, and default recommendation before coding.
2. **Vertical observable slices**: each deliverable is an end-to-end verified slice (not horizontal UI/API/DB splits).
3. **Independent review in fresh context**: "ready" requires E2E proof + review in a clean context without builder narrative bias.

## Showcase: semantic vault search

A self-contained, **offline** hybrid retrieval engine demonstrating the retrieval layer of a RAG
pipeline: local embeddings (`multilingual-e5-small`) + `sqlite-vec` vector store fused with FTS5
keyword recall via **Reciprocal Rank Fusion (RRF)**, grounded citations, and a **refuse-to-answer**
similarity threshold (anti-hallucination move).

→ **[`showcase/semantic-vault-search/`](showcase/semantic-vault-search/)** —
write-up, architecture diagram, and a runnable `demo.py` on an included sample corpus (no private data needed).

> **Not a production service.** This is an offline, historical prototype preserved for study.
> In production, daily knowledge lookup uses structured Markdown indexes and lexical search;
> agent memory uses PostgreSQL + pgvector with a different local embedding model. The demo is
> never deployed by this public repository.

## Guardrails, forged from incidents

Every rule in this system has an incident behind it. You can fork rules — you can't fork
scar tissue, so the *why* sits next to each:

The scripts below are curated examples. Their presence in this mirror does not mean they
are installed on the VPS; see [guardrail scope](docs/garde-fous.md) for limits and status.

| Rule | The incident behind it | Mechanical enforcement |
|---|---|---|
| **Sequential state ops** | A session hallucinated a merge and built analysis on phantom SHAs | `bin/jarvis-bash-guard.sh` blocks batched mutating git/gh commands |
| **No destructive prod DELETE** | An assistant emitted a ready-to-paste `DELETE FROM` on a client database | Evaluated scenario `09-no-delete-prod-client` + doctrine |
| **Tests green ≠ prod-ready** | Shipped a feature on unit tests alone that broke in mobile viewports | Notable Delivery gate (`jarvis-ship-check.py`) + E2E evidence |
| **Memory-size cap & write roots** | Memory bloated to "knows too much, arbitrates badly" | `bin/jarvis-memory-guard.sh` enforces file caps & headless write-roots |
| **Context-discipline watch** | Risky prod work attempted in a 3-hour marathon session | `bin/jarvis-context-watch.sh` alerts before entering the "dumb zone" |

Full operating doctrine: [**BEST-PRACTICES.md**](./BEST-PRACTICES.md).

## What's in the box

```
tests/doctrine/ ← 14 graded scenarios, weighted scoring, CI regression gate
bin/            ← guardrail hooks, delivery gates, bridge examples, historical retrieval tools
memory/         ← sanitized example templates; not the live memory store
claude-config/  ← path-scoped rule examples and a sanitized import template
showcase/       ← offline retrieval prototype on an included sample corpus
docs/           ← current VPS architecture + diagram, guardrail scope, competence map
AGENTS.md       ← standard cross-agent entry point (agents.md specification)
BEST-PRACTICES.md ← incident-forged operating doctrine
```

## Try it

All runnable pieces work on a fresh clone with no private data needed:

```bash
# 1. The eval harness — offline, deterministic, stdlib-only (what gates CI)
python3 tests/doctrine/runner.py

# 2. The delivery gate validator — test against a sample feature list
python3 bin/jarvis-ship-template.py sprint "Password reset flow" auth-app --out /tmp/sample.json
python3 bin/jarvis-ship-check.py /tmp/sample.json

# 3. The offline retrieval showcase — local embeddings + FTS5 over sample corpus
cd showcase/semantic-vault-search
pip install "sentence-transformers>=2.7" sqlite-vec numpy
python3 demo.py "how do I keep my services isolated?"
```

## License

**MIT** — see [`LICENSE`](./LICENSE). Curated public mirror; production code, credentials,
runtime data, and private vault notes are never committed here.
