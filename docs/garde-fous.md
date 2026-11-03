# Guardrail scope and limits

This document describes the guardrail examples included in the public mirror. Porunga's
production runtime is on the VPS; this repository is not the deployment source. A script
being present here does not prove it is installed or active on the VPS. The private
deployment inventory is authoritative for live hooks and settings.

## Current runtime boundary

- The VPS is the only Porunga execution host.
- No Porunga runtime component, hook, timer, or persistent store runs on the workstation.
- Public scripts below are examples or retained references unless the private deployment
  inventory says otherwise.
- These checks reduce accidental mistakes. They are not an operating-system sandbox or
  a replacement for access control, review, backups, and CI.

## Guardrails in this repository

| Component | Intended protection | Limit |
|---|---|---|
| [jarvis-bash-guard.sh](../bin/jarvis-bash-guard.sh) | Rejects chained mutating Git/GitHub CLI operations and selected destructive Git commands. Rejects commands it cannot parse. | Heuristic shell parsing; not a sandbox. Obfuscated or indirect commands can escape its view. |
| [jarvis-memory-guard.sh](../bin/jarvis-memory-guard.sh) | Checks configured write roots and limits growth of top-level memory files. | Applies only when the corresponding hook is installed and called with the expected payload. |
| [jarvis-context-watch.sh](../bin/jarvis-context-watch.sh) | Warns at transcript-size thresholds of 6, 11, and 16 MB. | Transcript size is only a rough proxy for model context. This retained script uses macOS-specific file-size syntax and must not be treated as an active VPS hook. |
| [jarvis-ship-check.py](../bin/jarvis-ship-check.py) | Fails a feature list when an item lacks an acceptance criterion, completion status, or evidence. | Validates the submitted checklist, not the product itself. |
| [tests/doctrine/](../tests/doctrine/) | Grades selected behavioural rules against recorded fixtures and gates CI. | Checks response text, not raw tool calls or production outcomes. |

## Bash guard contract

The Bash guard is a behavioural check for an agent hook. It is not a container,
chroot, seccomp profile, or permission boundary.

It is intended to reject:

- Multiple mutating Git or GitHub CLI operations chained in one command.
- Selected destructive Git operations, including force-push, hard reset, forced clean,
  branch deletion, interactive rebase, and amended commits.
- Empty, malformed, or unparseable hook payloads.

A normal standalone operation may still be allowed. The guard does not decide whether
the user authorized that operation. It also cannot reliably detect commands hidden in
encoded input, dynamic evaluation, or other indirect execution.

## Memory guard contract

The memory guard has two distinct checks:

1. A headless write-root boundary, when configured by the caller. Writes outside the
   allowed roots are blocked before the user bypass is considered.
2. File-count and file-size limits for selected top-level Markdown files in the
   configured memory directory.

A bypass only disables the growth limits; it does not disable the headless write-root
boundary. Both protections exist only when this hook is wired into the active agent
configuration.

## Delivery gate contract

The delivery validator checks that every feature has a description, acceptance
criterion, valid status, and evidence when marked done. It can prove that a checklist
contains evidence strings; it cannot prove those strings are truthful or that a real
end-to-end flow works. Use observable verification and an independent review for the
actual delivery.

## Layered protection

| Layer | What it contributes | What it cannot establish |
|---|---|---|
| Agent permissions | Blocks configured tool actions before execution | Cannot protect actions outside that agent configuration |
| Hooks and local scripts | Fast checks at selected tool boundaries | Cannot replace OS isolation or server-side policy |
| CI and repository permissions | Enforce checks and review rules on published changes | Cannot prove live services are healthy |
| VPS access controls and backups | Protect the deployed runtime and its data | Must be verified in the private deployment inventory |

Never describe a protection as active based only on its source file. Check the private
VPS configuration and the latest runtime verdict.
