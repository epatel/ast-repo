---
description: Check this project's agent docs (CLAUDE.md imports, card index, setup record) and report problems
---

Run the agent-docs checker on this project and report what it finds.

1. Run `python3 -I .claude/scripts/check-agent-docs.py .` from the project root. The script
   is read-only and needs Python 3.9 or newer.
2. If it prints `0 error(s), 0 warning(s)`, say so and stop.
3. Otherwise list each finding with its file. For each one, say what the fix is: a dead
   index link, a card that no index line points to, an `@` import of a card, a card that
   refers to another card, or a file over the size limit.
4. Do not change files unless the user asks you to fix the findings.

If `.claude/scripts/check-agent-docs.py` is missing, the `check-agent-docs.py` script is
not enabled in this project. Tell the user to run `ast add check-agent-docs.py`.
