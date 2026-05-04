# templates/

Files an adopting team copies into their own repositories.

The Shubin Framework is documentation, not a CLI or a scaffolder. There is
no `init` command and no template registry. When the framework prescribes
a concrete file — an Issue Template, an `AGENTS.md` for a specific
directory, a git convention — the canonical version of that file lives
here, and a team copies it into the right place in their own repo.

The current contents are:

- `.github/ISSUE_TEMPLATE/` — GitHub Issue templates plus `config.yml`
  and a copy guide. Two groups:
  - For **code repos**: `feature.md` (mandatory for P0/P1 single-part
    features), `feature-light.md` (P2 and minor work), `bug.md`,
    `sub-issue.md`. See `framework.md` ch. 3.2 for what the Feature
    template enforces and why.
  - For the **markdown repo (vault)**: `cross-cutting.md` (main Issue
    for features touching 2+ parts), `vault-task.md` (docs-only work),
    `adr-initiative.md` (large ADR efforts), `retro-item.md`
    (follow-ups from retros). See `framework.md` ch. 3.3 (Where Issues
    live) and 3.4 (Cross-cutting features) for placement rules.
- `knowledge/AGENTS.md` — instructions for the AI agent that maintains
  `knowledge/wiki/`. Belongs in the markdown repo of teams that adopt
  the optional knowledge module. See `knowledge.md` for the broader
  picture, including the auto-ingest workflow and Project integration.
- `conventions/VAULT_GIT_CONVENTIONS.md` — git commit convention for
  the markdown vault repo. Different from the base convention used in
  code repos: vault-specific tags (ADR, ARCH, JOURNAL, WIKI-AGENT, ...)
  and no version bumps.

Templates are intentionally not auto-installed. Copying is a deliberate
act: the team should read the file, understand it, and adapt it to their
naming and conventions before committing.
