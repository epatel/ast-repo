# ast-repo: a demo org repo for ast

This repo shows how an organisation can publish its agent setup with
[`ast`](https://github.com/epatel/ai-setup-tool). It holds named **sets** of tools. A project selects one set, and
`ast` installs the tools of that set into the project and keeps them current.

The content follows the patterns of https://ai.memention.net/setup: skills from
`epatel/agent-skills`, the project-setup skill, the standing-defaults card and the
agent-docs checker.

## What is in here

| Path | What it is |
| --- | --- |
| `ast-org.toml` | The sources and the sets. This is the file that `ast` reads. |
| `CARDS.md` | One line per card that says when to read it. `ast` copies these lines into a project's card index. |
| `cards/standing-defaults.md` | Card from ai.memention.net: Python venv and migration-path defaults. |
| `cards/agent-setup.md` | Card that tells agents and people how the installed tools are managed. |
| `cards/code-health.md` | Card from ai.memention.net: aihealth code-health feedback via hooks, MCP and a pinned venv install. |
| `cards/cards-instructions.md` | Card from ai.memention.net: how to build a project's card index (overview, commands, index, lazy-loaded cards). |
| `cards/hooks-over-prose.md` | Card from ai.memention.net: move checkable "always/never" rules from CLAUDE.md into `.claude/settings.json` hooks. |
| `cards/known-stack.md` | Card from ai.memention.net: name mainstream frameworks in one line instead of carding them. Experimental. |
| `cards/two-tier-claude-md.md` | Card from ai.memention.net: split feature `CLAUDE.md` files into a slim auto-loaded overview and an on-demand contract. |
| `cards/project-plan.md` | Card from ai.memention.net: a shared `project-plan.md` that keeps parallel agents on one goal and state. |
| `cards/feature-first-structured.md` | Card from ai.memention.net: features as directories, each a pipeline of hookable named nodes. |
| `cards/feature-first-advanced.md` | Card from ai.memention.net: peer hooks between features and per-request middleware, on top of feature-first-structured. |
| `scripts/check-agent-docs.py` | Checker from ai.memention.net for `CLAUDE.md` imports, the card index and the setup record. Read-only, standard library only. |
| `commands/check-agent-docs.md` | Slash command `/check-agent-docs` that runs the script and explains the findings. |

Skills are not copied into this repo. The sets refer to them in their own repos.

## The sets

```mermaid
flowchart TD
    default["default<br>double-check, review-agentic-setup, project-setup,<br>10 cards, checker script + command"]
    python["python-service<br>+ makefile-actions, state-machines, adt-types,<br>clean-modules, focused-refactor, minimal-repro"]
    docs["docs<br>+ ste-writing, backward-planning<br>− standing-defaults, code-health, known-stack,<br>feature-first-structured, feature-first-advanced cards"]
    fenix["proj-fenix<br>+ design-system<br>− minimal-repro"]
    default --> python
    default --> docs
    python --> fenix
```

A set is everything its `extends` parents give, plus its own `tools`, minus its own `exclude`.

| Set | Tools | For |
| --- | --- | --- |
| `default` | 7 | Baseline for every project |
| `python-service` | 13 | Python services and command-line tools |
| `docs` | 8 | Documentation and writing projects |
| `proj-fenix` | 13 | One project that needs its own mix |

## Use it in a project

```sh
cd my-project
ast init
ast use epatel/ast-repo        # this repo on GitHub; a git URL or a local path also works
ast select                     # list the sets
ast select python-service      # install that set
ast list                       # see each tool and the set it comes from
```

Then adjust the project without changing the org repo:

```sh
ast disable minimal-repro      # turn one tool of the set off
ast add ste-writing            # add a tool the set does not have
```

`ast` also lists the installed cards in the project's `CARDS.md` and adds `@CARDS.md` to
`CLAUDE.md`, so agents know when to read each card. The text of each line comes from
[`CARDS.md`](CARDS.md) in this repo.

## Change the org repo

Run `ast` inside this repo. It sees `ast-org.toml` and acts on the sets.

```sh
ast list                                   # every set, resolved
ast list --available                       # tools that no set uses
ast add focused-refactor --set docs        # add a tool to a set
ast disable double-check --set docs        # exclude an inherited tool
ast set new proj-atlas --extends docs --description "Project Atlas"
ast status                                 # check that every tool still exists
```

To add a tool of your own, put it in `cards/`, `commands/`, `scripts/`, `agents/` or
`skills/<name>/SKILL.md`, then run `ast add <name>`.

Commit and push. Each project gets the change when it runs `ast update`; before that,
`ast status` in the project shows what the update will install, change and remove.

A project reads `ast-org.toml` from git, not from your working tree, so a change that is
not committed and pushed does not reach any project.
