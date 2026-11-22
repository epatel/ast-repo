# Apply the Two-Tier CLAUDE.md pattern

You are a coding agent. Split large per-feature `CLAUDE.md` files into a slim overview that always loads and a full contract that loads on demand. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Apply this only after feature-first docs exist.
2. For each feature `CLAUDE.md` over about 40 lines, split it into `CLAUDE.md` and
   `CLAUDE_full.md`.
3. Point to `CLAUDE_full.md` by plain path with a "read when…" condition. Never write
   `@CLAUDE_full.md`: that is an import, and it loads the file every time.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/two-tier-claude-md.md}}
