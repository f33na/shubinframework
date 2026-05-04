# Vault Git Convention

> Standard version: 1.0.0

This convention applies to the **vault repository** of a product —
the markdown repo holding decisions (ADRs), patterns, architecture,
glossary, journal, and knowledge wiki. For code repositories of the
same product, use the base Git Convention.

A vault is documentation, not code. It has no build, no tests, no
runtime, no releases. Standard tags like FEATURE / FIX / HOTFIX / MAJOR
assume a working artifact that breaks or improves — not relevant here.
The verbs in a vault are different: **decide**, **record**, **refine**,
**ingest**, **supersede**.

---

## Commit format

```
[TASK-ID/TAG] Short description of the change
[TAG] Short description of the change
```

- **TASK-ID** — optional. Used when a vault-task Issue exists in the
  vault repo (e.g. `VAULT-12` from a `vault-task.md` Issue Template).
  Most vault commits don't have one.
- **TAG** — required. See tag table below.
- **Description** — up to 72 characters, imperative mood, no period at
  the end, in the team's chosen language (English by default).

If more context is needed, add a body after a blank line. ADR commits
in particular benefit from a body explaining what the ADR decides.

---

## Tags

| Tag          | What it covers                                              | Where              |
| ------------ | ----------------------------------------------------------- | ------------------ |
| `ADR`        | New ADR added                                               | `decisions/`       |
| `SUPERSEDE`  | New ADR replacing an existing one (old marked Superseded)   | `decisions/`       |
| `ARCH`       | Changes to ARCHITECTURE.md (boundaries, invariants, layout) | root               |
| `STACK`      | Changes to STACK.md                                         | root               |
| `GLOSSARY`   | Changes to GLOSSARY.md (additions, refinements, removals)   | root               |
| `TEAM`       | Changes to TEAM.md                                          | root               |
| `PATTERN`    | New pattern or update to existing                           | `patterns/`        |
| `JOURNAL`    | Entry in journal (feedback, incident, retro)                | `journal/`         |
| `RAW`        | External source added to knowledge raw                      | `knowledge/raw/`   |
| `WIKI`       | Manual edit in knowledge wiki (human correction)            | `knowledge/wiki/`  |
| `WIKI-AGENT` | Automated wiki compile commit (AI-generated, PR-only)       | `knowledge/wiki/`  |
| `GUIDE`      | Long-form guide added or updated                            | `guides/`          |
| `AGENTS`     | Changes to AGENTS.md or knowledge/AGENTS.md                 | root, `knowledge/` |
| `DOCS`       | README, CONTRIBUTING, other meta docs                       | various            |
| `STYLE`      | Formatting, typos, link fixes (no semantic change)          | various            |
| `I18N`       | Translation changes                                         | `i18n/`            |
| `CHORE`      | .gitignore, gitattributes, CI configs, dependencies         | various            |

---

## Tag selection rules

**Multiple files in one commit — pick the dominant tag.** A commit that
adds an ADR and updates GLOSSARY for a new term introduced by the ADR
is `[ADR]`, not `[ADR/GLOSSARY]`. The glossary update is supporting
work; explain it in the body.

**Don't combine unrelated changes.** If you're tempted to write
`[GLOSSARY/JOURNAL]`, those are two separate commits.

**ADR commits include the ADR number in the description:**

```
[ADR] ADR-0024 OAuth library choice
[SUPERSEDE] ADR-0031 supersedes ADR-0024 (rotation strategy changed)
```

**WIKI-AGENT is reserved for the wiki maintainer agent.** Humans never
use this tag manually. The agent commits to a branch and opens a PR;
review and merge are human steps. Manual corrections to the wiki use
`[WIKI]`:

```
[WIKI-AGENT] weekly compile 2026-04-29

Updated domains: auth, billing, onboarding.
Ingested 4 new raw sources, 2 ADRs, 7 closed Issues.
```

```
[WIKI] Move session-management entries from auth.md to infra.md
```

**JOURNAL entries are append-only by convention.** Editing an existing
journal entry is rare — usually only to fix typos right after creation
(`[STYLE]`). If you must change an old journal entry substantively,
explain why in the commit body.

---

## Branches

- **No branch.** Direct commit to main is acceptable for small
  documentation changes — typo fixes, single glossary terms, single
  journal entries. The vault is not production code; gating every
  small edit behind a PR adds friction without much benefit. Decide
  with your team if you prefer always-PR.
- **Feature branch with TASK-ID.** When a vault-task Issue exists, the
  branch name matches: `VAULT-12`. Commits use the full format:
  `[VAULT-12/GUIDE] Add troubleshooting guide for auth failures`.
- **Feature branch without TASK-ID.** ADRs typically don't need a
  tracking Issue (the PR is the discussion). Use a descriptive branch
  name: `adr-0024-oauth-library`. Commits use the short format:
  `[ADR] ADR-0024 OAuth library choice`.
- **Wiki agent branches.** The agent commits to
  `wiki/auto-ingest-YYYY-MM-DD` and opens a PR titled
  `wiki: weekly compile YYYY-MM-DD`.

---

## ADR lifecycle in commits

A typical ADR's git history looks like:

```
[ADR] ADR-0024 OAuth library choice         (initial PR)
[STYLE] Fix typo in ADR-0024                (post-merge fix, allowed)
[SUPERSEDE] ADR-0031 supersedes ADR-0024    (much later, in ADR-0031's PR)
```

After an ADR is merged, its body is updated only in two ways:

1. **Status change** from "Accepted" to "Superseded by ADR-NNNN" — done
   in the SUPERSEDE commit, in the same PR that adds the new ADR.
2. **Pure typo fixes** — `[STYLE]` tag, no semantic change.

Anything beyond that means writing a new ADR.

---

## No version bumps

The vault is not versioned. There are no MAJOR/MINOR/PATCH tags, no
RELEASE commits, no git tags. The vault is a living document set, not
a release artifact. Branches and commits accumulate; old states are
recovered through git history if needed.

If the vault structure itself needs a major reorganization (rare —
prefer gradual ADR-driven change), do it as one or more `[ARCH]`
commits within a single PR, with a clear ADR explaining why.

---

## Examples

```bash
# ADR work
[ADR] ADR-0024 OAuth library choice
[ADR] ADR-0025 Token storage location
[SUPERSEDE] ADR-0031 supersedes ADR-0024 (rotation strategy changed)

# Architecture
[ARCH] Mark notifications module boundary
[ARCH] Add invariant: no direct DB access from frontend

# Stack and glossary
[STACK] Replace Heroku with Fly.io for staging
[GLOSSARY] Add term "idempotency key"
[GLOSSARY] Refine definition of "settlement window"
[TEAM] Update on-call rotation for Q2

# Journal
[JOURNAL] feedback: customer X interview on onboarding
[JOURNAL] incident: 2026-04-12 token leak postmortem
[JOURNAL] retro: 2026-Q1

# Patterns
[PATTERN] Add error-handling pattern for HTTP retries
[PATTERN] Update form-validation pattern with new examples

# Knowledge module
[RAW] Stripe Connect best practices article
[RAW] Customer interview transcript 2026-04-22
[WIKI-AGENT] weekly compile 2026-04-29
[WIKI] Move session-management entries from auth.md to infra.md

# Other
[GUIDE] Add troubleshooting guide for auth failures
[AGENTS] Tighten rule on raw-folder immutability
[DOCS] Update README with new module list
[STYLE] Fix broken links in ADR-0017
[I18N] Sync ru translations of glossary
[CHORE] Update .gitattributes for markdown linguist

# With TASK-ID (vault-task Issue exists)
[VAULT-12/GUIDE] Add troubleshooting guide for auth failures
[VAULT-15/I18N] Translate ARCHITECTURE.md to ru
[VAULT-21/PATTERN] Document new caching pattern
```

---

## Quick reference

```
Branch / task?
  YES → [TASK-ID/TAG] Description
  NO  → [TAG] Description

Most common tags by area:
  decisions/        → ADR, SUPERSEDE
  ARCHITECTURE.md   → ARCH
  STACK.md          → STACK
  GLOSSARY.md       → GLOSSARY
  TEAM.md           → TEAM
  patterns/         → PATTERN
  journal/          → JOURNAL
  knowledge/raw/    → RAW
  knowledge/wiki/   → WIKI (human), WIKI-AGENT (AI compile)
  guides/           → GUIDE
  AGENTS files      → AGENTS
  i18n/             → I18N
  README, CONTRIB.. → DOCS
  formatting only   → STYLE
  configs           → CHORE

The vault is not versioned. No MAJOR/MINOR/PATCH. No RELEASE.
```
