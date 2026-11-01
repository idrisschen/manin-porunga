# AI engineer signals

> A concise map from this public repository to the engineering practices it demonstrates.

Porunga's production runtime is on one VPS. The Mac does not host Porunga runtime
components, hooks, timers, or persistent state. This repository is a sanitized public mirror, not the
deployment source. The live system's high-level boundary and timer inventory are in
[Current Porunga architecture](architecture.md).

## Competency → evidence

| Competency | Evidence in this repository | What it demonstrates |
|---|---|---|
| **Context engineering** | HOT / WARM / COLD examples, path-scoped rules, and [AGENTS.md](../AGENTS.md) | Deliberate context selection and clear source-of-truth boundaries. The public files are examples; live memory and runtime wiring stay private. |
| **VPS operations** | [Current architecture](architecture.md) | One execution host, visible scheduled work, and a distinction between checked runtime state and files that merely exist in Git. |
| **Retrieval engineering** | [Offline semantic-search showcase](../showcase/semantic-vault-search/) | A runnable hybrid retrieval prototype with local embeddings, FTS5, Reciprocal Rank Fusion, and a refusal threshold. It is an educational artifact, not a live VPS service. |
| **Model orchestration** | Agent-role examples and [BEST-PRACTICES.md](../BEST-PRACTICES.md) | Independent review and model diversity as design practices. The public mirror does not disclose private agent endpoints or provider wiring. |
| **Delivery engineering** | [jarvis-ship-check.py](../bin/jarvis-ship-check.py) and [jarvis-ship-template.py](../bin/jarvis-ship-template.py) | Evidence-based completion, observable vertical slices, and independent review before calling work ready. |
| **LLM evaluation** | [tests/doctrine/](../tests/doctrine/) | Behavioural scenarios with weighted assertions, category aggregation, regression detection, and a CI gate. |
| **Safety and reliability** | [jarvis-bash-guard.sh](../bin/jarvis-bash-guard.sh), [jarvis-memory-guard.sh](../bin/jarvis-memory-guard.sh), and [jarvis-context-watch.sh](../bin/jarvis-context-watch.sh) | Mechanical checks around destructive shell operations, memory growth, and long sessions. See [guardrail scope](garde-fous.md) for limits and deployment status. |

## Reproducible numbers

These values describe committed fixtures or a small sample-corpus demo, not production
capacity or live-model performance.

| Measure | Value | Source |
|---|---:|---|
| Doctrine scenarios | **14** across 6 categories | [tests/doctrine/](../tests/doctrine/) |
| Recorded offline baseline | **100% (14/14)**, including 8/8 critical scenarios | [tests/doctrine/report.md](../tests/doctrine/report.md) |
| Offline suite latency | **about 8 ms** | Same recorded baseline |
| Retrieval demo model | multilingual-e5-small, 384 dimensions | [Showcase README](../showcase/semantic-vault-search/README.md) |
| Retrieval demo latency | **about 32–65 ms warm**; **about 10 seconds cold model load** | Historical sample-corpus benchmark on an Apple Silicon laptop; not a VPS benchmark |

The offline baseline grades recorded reference responses. It is a deterministic
regression check, not proof of live-model capability. The optional live and judge
modes depend on the configured model and may vary.

## Honest limits

- This is a personal system, not a multi-tenant SaaS. The public repository contains
  sanitized examples, not production customer data or the full production codebase.
- The VPS is the only Porunga runtime. A machine used to access a remote session is
  not an execution host.
- The 14-scenario suite covers selected high-consequence rules. It does not prove
  universal correctness.
- The deterministic harness grades text and does not inspect raw system calls.
  Mechanical shell guards add a separate execution-time check.
- The retrieval benchmark uses a tiny public fixture corpus and old laptop hardware.
  It says nothing about current VPS latency.

## Common questions

**Why keep a vector-search demo if production uses another retrieval approach?**

The demo is a reproducible record of an earlier design. The current production
knowledge lookup uses structured Markdown indexes and deterministic lexical search.
Keeping the prototype makes the trade-offs concrete without implying it is deployed.

**How is the runtime status verified?**

Check the live systemd timers and running services on the VPS. A versioned unit or
script in any repository is not evidence that it is installed or healthy.

**What does the evaluation suite establish?**

It shows that selected behavioural rules still match their recorded fixtures and
that regressions in those assertions fail CI. It does not replace review of actual
agent actions or production outcomes.
