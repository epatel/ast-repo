These steps add the documentation patterns of https://ai.memention.net/setup to the tools
that `ast` installed. `ast.lock` records the installed tools. Do not write
`.claude/agent-setup.json`.

### A. Assess the project

1. Read the `CLAUDE.md` of the project (if any), the `README.md`, and the manifest
   (`package.json`, `pubspec.yaml`, `Cargo.toml`, `go.mod`, `pyproject.toml` or similar).
2. Walk the top-level directory structure.
3. Note the language and framework, the approximate size (files, modules), and if the
   project has a `cards/` directory or a `CLAUDE.md` in each feature.
4. If the project has agent docs, run `python3 -I .claude/scripts/check-agent-docs.py .`
   now (it is read-only) and use its output.
5. Decide the mode:
   - **setup**: little or no agent structure exists.
   - **update**: the project already has agent structure: `CLAUDE.md` with cards, skills
     in `.claude/skills/`, a project plan, or similar.
6. In update mode, be careful. The structure holds decisions and project knowledge.
   - Do not apply patterns over it or replace whole files. Extend and refresh.
   - Treat each file as intentional until you can show that it is not.
   - Run the `review-agentic-setup` skill in report-only mode
     (`/review-agentic-setup --report-only`). It writes nothing. Put the changes that it
     proposes in the scoping summary.
   - If `.claude/agent-setup.json` exists, an earlier setup copied tools in by hand. Tell
     the user, and offer `ast prompt adopt` after these steps.

### B. Present the scope

Show the user a summary, and ask which patterns to apply:

- the mode (setup or update), and why
- two or three sentences about what you found: stack, size, state of the docs
- for each pattern in the table: if you recommend it, with one line of reason
- for each recommended pattern that needs tools (the `## Requires` table of its pattern):
  what detection found. Detection is read-only (`ast prompt install-tools`, step 1).
  Install nothing yet.

| Pattern | Prompt | What it does | Best for |
|---|---|---|---|
| Known Stack ⚗️ | `ast prompt known-stack` | Names the standard frameworks and patterns in one line each in `CLAUDE.md` | Projects built mostly from mainstream frameworks used the standard way |
| Context Cards | `ast prompt context-cards` | Small cards that load on demand, indexed from `CLAUDE.md` | A project where `CLAUDE.md` grows large or agents need domain context |
| Feature-First Architecture ⚗️ | `ast prompt feature-first` | Documents code organised per feature as a pipeline of named nodes with hooks | Code that already has an explicit pipeline, middleware or hook engine |
| Feature-First Advanced ⚗️ | `ast prompt feature-first-advanced` | Peer features in both directions, hooks bound at runtime, full middleware model | Feature-first projects where features hook into each other |
| Two-Tier CLAUDE.md | `ast prompt two-tier` | Splits a feature `CLAUDE.md` into a slim overview and a full contract on demand | Feature-first projects with features of 3 or more nodes |
| Shared Project Plan | `ast prompt project-plan` | One `project-plan.md` with the goal and live state that every agent reads first | Work spread over subagents, worktrees, or autonomous runs |
| Standing Defaults | `ast prompt standing-defaults` | Python venv always, and a migration path for stored data | A project that runs Python or stores data |
| Hooks Over Prose | `ast prompt hooks-over-prose` | Moves mechanical "always/never" rules into settings and hooks | Instruction files with rules that a script can check |
| Code Health Feedback ⚗️ | `ast prompt code-health` | Installs aihealth, pinned, and wires it into hooks and MCP | Projects where agents regularly change code |
| Anthropic's skills | `ast prompt anthropic-skills` | Enables plugins from Anthropic's skills marketplace for the project | Web frontends, office documents, work on the Claude API |

Do not continue until the user confirms the scope.

### C. Apply the confirmed patterns

For each pattern that the user confirmed, run its prompt command and follow the output.

- Apply Known Stack before Context Cards.
- Apply Feature-First before Feature-First Advanced and Two-Tier.
- When a pattern needs a tool, follow `ast prompt install-tools`. Never install anything
  without the user's yes, and let the user choose where.
- The patterns document code that exists. They do not restructure or refactor it.
- In update mode, apply also the changes from the review report that the user confirmed.

### D. Verify

1. Run `python3 -I .claude/scripts/check-agent-docs.py .`. Fix each ERROR that the
   confirmed scope covers. Report the ERRORs that stay, and do not fix them unasked. Judge
   each WARN. The warning about a missing setup record is expected: `ast.lock` replaces it.
2. Open each new file cold and confirm that it can be read on its own.
3. Confirm that the index entries in `CLAUDE.md` name the situation that must trigger the
   card, not only its topic.
4. Put in your final report: the files created and the patterns applied, the tools used or
   installed and where, the tools skipped and what stays inactive because of them, and the
   checker output.

### Notes

- If the project also serves other agents that read `AGENTS.md`, keep the shared content
  (overview, commands, card index) in `AGENTS.md`, and add a thin `CLAUDE.md` that imports
  it with `@AGENTS.md` only if you need lines for Claude alone.
- Extend cards, feature docs and skills that exist. Never overwrite them.
