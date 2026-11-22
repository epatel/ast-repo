# Context Cards — Setup

A system that compresses project knowledge into small, lazy-loaded reference cards. It replaces one giant `CLAUDE.md` that holds everything, and it replaces grep-and-pray.

Reference this file from a project's `CLAUDE.md` to bootstrap a card index for that project.

## The model

- **Overview** — 2-3 sentences in `CLAUDE.md` above the index. Always loaded. What the product does, who it is for, the one or two stack facts that change every answer ("Rails 7 + Postgres", "Flutter + Riverpod"). Too small to lazy-load and too universal to make conditional, so it is not a card.
- **Commands** — a short block in `CLAUDE.md` under the overview: the exact commands to set up, run, test (including a single test), lint, and build. Needed in nearly every coding session, so it is always loaded rather than a card. If there is a Makefile or task runner, list its targets instead of the raw commands.
- **Index** — always loaded. One line per card: `- [name](cards/name.md) — trigger`.
- **Card** — loaded on demand when its trigger matches the situation.

Card shape:

```markdown
# <name>

<one-line what-this-is>

<body>
```

No frontmatter. The trigger lives in the **index entry**, where the LLM actually reads it to decide whether to open the card. A copy of it inside the card adds noise without adding function. Add structured metadata only when tooling exists that consumes it.

Use mermaid (```` ```mermaid ```` fenced blocks) for diagrams — flows, component connections, state transitions — instead of ASCII art. Mermaid renders inline; ASCII does not.

**The one rule: cards are self-contained.** No "see also card X" links. If two cards share content, hoist it into a third card and let the index point to all three. Transitive loading recreates the wiki problem this is meant to avoid.

## Three card types

| Type | Captures | Example |
|---|---|---|
| **domain** | A bounded slice of the system | `auth`, `billing`, `search-index` |
| **feature** | A specific capability or flow within a domain | `magic-link-login`, `subscription-renewal` |
| **decision** | A choice plus rationale (mini-ADR) | `postgres-over-mongo`, `monorepo-layout` |

Cards live under `cards/` flat — the index section shows the type, not the path.

## Coexistence with per-feature CLAUDE.md

When features have their own directory with a co-located `CLAUDE.md` (e.g., `lib/features/<name>/CLAUDE.md`), **do not create a separate domain or feature card** for that feature. Claude Code auto-discovers the co-located CLAUDE.md. It should be the single source of truth for that feature's contract.

Cards are for content with no natural home directory:
- **Architecture** — system topology, data flow (spans all features)
- **Decisions** — rationale for choices (not owned by one feature)
- **Shared patterns** — hook-wiring semantics, engine API (used by all features)

**Rule of thumb:** If the content describes one feature's internal contract (nodes, context keys, hook guidance, boundary), put it in that feature's co-located CLAUDE.md. If it describes how multiple features interact or a system-wide concern, it belongs in a card.

## The architecture card

One additional, **singleton** card belongs at the top of the index: `architecture`. It captures how components connect, data flows end to end, and the deployment shape — content that does not fit any single domain because it spans them.

Trigger: cross-domain work, dataflow debugging, or onboarding.

This is the card most likely to drift into wiki-article territory because it is cross-cutting by nature. Hold the self-containment rule strictly: describe connections in flat prose ("auth issues a JWT that billing verifies"), never `see cards/auth.md`. If it grows past ~150 lines, split by sub-trigger (`architecture-dataflow`, `architecture-deploy`) rather than letting it become a hub.

## Populating cards

The step-by-step procedure for an existing project (orient, discover domains, features and decisions, build the index, verify) is a separate document: `https://ai.memention.net/cards/cards-procedure.md`. Fetch it when building cards for a project for the first time, or when restructuring them after a major refactor. This file is the model and the rules for adding or changing a single card.

## Maintenance

- New non-obvious logic lands → new feature card.
- A PR debates an architectural choice → new decision card.
- A card goes stale → update or delete it. Do not append `DEPRECATED:` notes. Git history is the audit log.
- A card grows past ~150 lines → split by sub-trigger or it has become a wiki article.

## How to use from a project

### Bootstrap (one-time)

In a session in the target project, prompt:

```
See https://ai.memention.net/cards
Build index cards and add index entries to CLAUDE.md
```

The agent reads this file and fetches the procedure (`https://ai.memention.net/cards/cards-procedure.md`). It runs the procedure's Steps 0–5 against the project, writes the cards under `cards/`, and appends the index to `CLAUDE.md`.

### Steady state

After bootstrap, the project's `CLAUDE.md` only needs the **index itself** — not a permanent `@`-import of this file or the procedure. Both are build-time scaffolding. Once cards exist, the index of markdown links is self-explanatory at runtime.

Read this file again on demand when **extending** the system: adding cards for a new domain, retiring stale ones. Fetch the procedure again when restructuring after a major refactor.
