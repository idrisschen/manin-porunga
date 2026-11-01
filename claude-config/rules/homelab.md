---
paths:
  - "**/Homelab/**"
  - "**/workspace/**"
  - "**/manin-porunga/**"
  - "**/Projects/manin-porunga.md"
canonical_sources:
  - "Private Homelab index and current project notes"
  - "Private Porunga deployment inventory"
last_reviewed: 2026-11-01
---

# Rule — VPS runtime and internal infrastructure

Loaded for Homelab, workspace, and Manin Porunga work. This public rule is
sanitized; live host identities, addresses, paths, and credentials stay in private
configuration.

- **Porunga runtime**: use the public-facing name **VPS**. It is the only Porunga
  execution host. No Porunga runtime component, hook, timer, memory database, or
  persistent runtime state runs on the Mac.
- **Repository authority**: `manin-porunga` is a curated public mirror, not the
  production source or deployment target. Production code and deployment
  configuration live in the private canonical repository.
- **Verify before an external operation**: before recommending or performing a
  deployment, migration, DNS change, rollback, or credential change, read the
  current private project notes and deployment inventory. A remembered target or
  a checked-in example is not evidence of live state.
- **Project-specific deployment targets**: use the target recorded for that
  project. Do not infer that an app belongs on the VPS, a platform, or another
  machine from this rule.
- **Credentials and private data**: load them only through the configured private
  mechanism on the VPS. Never copy them into this repository, logs, prompts, or
  public documentation.
- **Access**: use the approved remote-access path already configured for the
  operator. If the route or host identity cannot be verified, stop and report the
  access blocker; do not disable host-key or identity checks.
- **Sequential state changes**: one mutating Git or infrastructure command at a
  time, followed by an isolated verification. Preserve existing work from other
  sessions.
