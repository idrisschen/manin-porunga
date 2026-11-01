# AGENTS.md — Manin Porunga (Public Curated Mirror)

Entry point for **any** coding agent (Claude Code, Antigravity CLI, Codex, Grok, Cursor, Amp, …)
working in this repository, conforming to the [agents.md](https://agents.md) standard.

## What this is

**Manin Porunga** is the curated public mirror of **Porunga**, the personal AI agent
control room created by Idriss Bhugeloo (Manin Studio). The production runtime is on one
VPS. No Porunga runtime component or background automation runs on the Mac.

It documents selected engineering patterns from an autonomous agent system built on LLMs:
- **Durable Markdown memory** with strict **HOT / WARM / COLD** tiers.
- **Multi-agent role & model diversity** (independent agent roles and model families; private runtime wiring is not published here).
- **Offline LLM-evaluation harness** (`tests/doctrine/`) with 14 graded behavioural scenarios gating CI.
- **Notable Delivery Contract** (`bin/jarvis-ship-check.py`): mechanical gating requiring structured hypotheses, observable vertical slices, and independent fresh-context review.
- **Mechanical guardrails**: `PreToolUse` bash hook blocking batch mutating git commands and rm -rf, memory file caps, and dumb-zone context watchers.
- **Hybrid RAG & the Post-RAG pivot**: local vector retrieval (`sqlite-vec` + e5 + FTS5 + RRF) showcase, and the architectural lesson of why production evolved to deterministic structured indexes (`_vault-index.md` + ripgrep).

## Source of truth & hierarchy

| Tier / Location | Role | Authority |
|---|---|---|
| **Git repo (here)** | Curated public mirror, sanitized doctrine, eval harness, tools | Source for this public example only; never the production deployment source |
| **Private canon** | Private Git repo + private knowledge base | Absolute source of truth for production code, configuration, and private doctrine |
| **Notion** | Archive of machine outputs (briefs, watchtower, session recaps) | Never a source of truth |

## Hard rules — read before touching anything

1. **Deterministic validation**: every change to doctrine or scenarios must keep `python3 tests/doctrine/runner.py` at **100% pass rate** (14/14 pass, 8/8 critical).
2. **Never commit secrets**: no API keys, private tokens, private client names, or personal finance details. This repository is public.
3. **Commit by explicit pathspec only**: `git commit <file1> <file2> -m "..."`. Never `git add -A`, `-a/-am`, or `git commit .`.
4. **Sequential state operations**: one mutating git/deploy command at a time, followed by isolated verification. Never batch mutating commands.
5. **Shell scripts use `set -uo pipefail` without `-e`**: deliberate design to avoid SIGPIPE breakage on early-exit pipes (`grep -q`, `head`).
6. **No fluff / No theater**: documentation and code must be dense, clear, and actionable.
7. **Do not infer deployment from this tree**: a file or unit example here does not prove it is installed or active on the VPS. Check the private deployment inventory for live state.
8. **Keep public documentation sanitized**: use the public-facing term `VPS`; omit provider, host identity, network addresses, private paths, and credentials.

## Key directories

- `bin/`: curated executable examples — guardrails, delivery gate tools, bridge examples, token observability reference, and historical retrieval scripts. Presence here does not mean a tool is active in production.
- `claude-config/`: path-scoped rules (`rules/`) demonstrating contextual loading based on active workspace path.
- `memory/`: sanitized example templates only; not the live memory store or production doctrine.
- `tests/doctrine/`: the 14-scenario behavioural eval harness (`runner.py`, `scenarios/`, `fixtures/`).
- `showcase/semantic-vault-search/`: runnable offline retrieval prototype on a public sample corpus; not deployed on the VPS.
- `docs/`: current VPS architecture, guardrail scope, and AI-engineering evidence.
- `BEST-PRACTICES.md`: complete operating doctrine distilled from real production incidents.
