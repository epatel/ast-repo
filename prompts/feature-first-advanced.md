# Apply the Feature-First Advanced pattern (experimental)

You are a coding agent. Document features that hook into each other, hooks that bind at runtime, and middleware. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Apply this only on top of the feature-first pattern (`ast prompt feature-first`), when
   features hook into each other or hooks bind at runtime.
2. If static hooks in one direction already cover the project, tell the user and stop.
3. Extend the feature `CLAUDE.md` files with what the pattern below describes. Document
   only what exists in the code. Do not refactor code unless the user asks.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/feature-first-advanced.md}}
