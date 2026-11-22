# Context Cards — Procedure

How to populate context cards for an existing project. Context cards are small, lazy-loaded reference files under `cards/`. A one-line index in `CLAUDE.md` tells the agent when to open each one. Run this procedure once to bootstrap a project, and again when restructuring after a major refactor.

## Rules every card follows

- **Shape.** `# <name>`, a one-line what-this-is, then the body. No frontmatter: the trigger lives in the index line, where the agent reads it to decide whether to open the card.
- **Self-contained.** No "see also card X" links. If two cards share content, hoist it into a third card and let the index point to all three.
- **Flat.** Cards live under `cards/` with no subdirectories. The index section conveys the type: architecture, domain, feature or decision.
- **No duplicate homes.** A feature with its own directory and a co-located `CLAUDE.md` (e.g. `lib/features/<name>/CLAUDE.md`) gets no card. That file is its contract. Cards are for content with no natural home directory: architecture, decisions, patterns shared by several features.
- **Small.** Past ~150 lines, split by sub-trigger (`architecture-dataflow`, `architecture-deploy`).
- **Diagrams** are mermaid (```` ```mermaid ```` fenced blocks), not ASCII art.

## Step 0 — Orient

Before discovering individual domains, produce the always-loaded and cross-cutting context:

1. **Overview** — write 2-3 sentences directly into `CLAUDE.md` above where the index will go. What the product does, who uses it, key stack/runtime facts.
   Below it, a **Commands** block. Take the commands from the Makefile, `package.json` scripts, CI config, or README. Before you write a cheap command down (`--help`, the test command), run it to confirm it works.
2. **Architecture card** — one card capturing components, connections, data flow, deployment shape. Content sources: top-level README, `docker-compose.yml` / `k8s/`, deployment configs, the dependency graph between top-level modules. Describe connections in flat prose ("auth issues a JWT that billing verifies"), never `see cards/auth.md`.
   `CLAUDE.md` can have a **Known stack** section (one line per standard framework or pattern). If the architecture is a textbook instance of it, skip this card. Likewise skip domain cards for layers those lines already cover. Card only what the code and the model's own knowledge cannot supply.

## Step 1 — Discover domains

- Walk top-level structure: `src/`, services, packages, modules.
- Group by **bounded context**, not by layer. `auth/` is a domain. `controllers/` is not.
- Target 5–15 domain cards. With too few, each card becomes a mini-wiki. With too many, the index bloats.

For each domain card, capture:
- **Purpose** — one sentence.
- **Public surface** — the entry points other domains call.
- **Key types and concepts** — the 3–7 names that recur throughout.
- **Owns / does not own** — explicit scope boundary.

## Step 2 — Discover features within domains

Look for: user-facing flows, named jobs, scheduled tasks, API endpoints with non-trivial logic.

A feature deserves a card when it has invariants, ordering constraints, or non-obvious failure modes. Skip CRUD that is "just save the thing."

For each feature card, capture:
- **Trigger** — the user action or event that starts it.
- **Steps** — the actual flow, 3–10 bullets.
- **Invariants** — what must remain true throughout.
- **Failure modes** — known ways it breaks.

## Step 3 — Distill decisions

Sources to mine:
- Existing `docs/adr/` or `docs/decisions/` directories.
- README sections titled "Why X" or "Architecture".
- Long commit messages explaining tradeoffs.
- Comments containing "because", "we chose", "instead of".
- PRs with substantial review discussion.

For each decision card, capture:
- **The choice** — one sentence.
- **Alternatives considered** — bullets.
- **Why this won** — the deciding constraints.
- **When to revisit** — the signal that would invalidate this decision.

## Step 4 — Build the index

Append to the project's `CLAUDE.md` (or create `CARDS.md` and import it):

```markdown
## About this project

<2-3 sentences: what it is, who it's for, key stack facts>

## Commands

- Setup: `make setup`
- Run: `make start`
- Test: `make test` (one test: `pytest tests/test_auth.py::test_login`)
- Lint: `make lint`

## Cards

### Architecture
- [architecture](cards/architecture.md) — cross-domain work, dataflow, onboarding

### Domains
- [auth](cards/auth.md) — anything touching login, sessions, identity
- [billing](cards/billing.md) — subscriptions, invoices, payment flows

### Features
- [magic-link-login](cards/magic-link-login.md) — passwordless email login flow
- [subscription-renewal](cards/subscription-renewal.md) — recurring charge + grace period

### Decisions
- [postgres-over-mongo](cards/postgres-over-mongo.md) — primary datastore choice
- [monorepo-layout](cards/monorepo-layout.md) — package boundaries and tooling
```

The trigger phrase after the em-dash is the **only** thing the LLM uses to decide whether to open the card. Phrase it as the situation, not the topic: "anything touching login" beats "auth module".

Use plain markdown links, never `@cards/name.md`. An `@` reference is an import that loads the card into every session. That defeats lazy loading.

Add a maintenance rule below the index so the cards stay current without anyone remembering to ask:

```markdown
When you commit a change that adds non-obvious logic, settles an architectural choice,
or makes a card above wrong, update or add the card in that commit and keep this index
in sync. Proposed diffs and answers need no card edits. Cards stay self-contained: no
card links to another.
```

Scope it to commits. A rule phrased "in the same change" makes agents add card edits to every diff they propose, even for a quick question.

## Step 5 — Verify

- Every card is readable in isolation. Open each one cold. If it requires another card to make sense, fix it.
- Index entries name the **trigger**, not the topic.
- Total index stays under ~150 lines so it pays its always-loaded cost.
- No card references another card by name.

After this run, the project's `CLAUDE.md` needs only the index, not an `@`-import of this procedure.
