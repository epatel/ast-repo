# Apply the Feature-First Architecture pattern (experimental)

You are a coding agent. Document code that is organised per feature as a pipeline of named nodes with hooks. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Recommend the pattern only when the code already has an explicit pipeline, middleware
   or hook engine: named steps, and a way for other code to attach before or after them.
   Show the user where that is (file and symbol). If there is none, recommend that the
   user declines, and stop. Plain modules or MVC layers are not pipelines.
2. Find the features or modules that map to the pipeline model.
3. Create a `CLAUDE.md` for each feature. Document only nodes, context keys and hooks that
   exist in the code, each traceable to a file. Never invent nodes or hook points.
4. This is a documentation pattern. Do not refactor code unless the user asks.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/feature-first-structured.md}}
