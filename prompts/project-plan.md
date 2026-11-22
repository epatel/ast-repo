# Apply the Shared Project Plan pattern

You are a coding agent. Create one `project-plan.md` with the goal and the live state, which every agent reads first and updates. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Create `project-plan.md` at the repo root with the sections Goal, Non-goals,
   Milestones, Decisions, Current state, and Open questions. Fill them from what you know
   about the project. Ask the user for the goal if it is not clear.
2. Reference it from `CLAUDE.md` with `@project-plan.md`. This file is imported with `@`,
   unlike a card: every session needs the current goal.
3. Reference it from each subagent or worktree spawn prompt in the project, so that every
   agent reads it first and updates it before it finishes.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/project-plan.md}}
