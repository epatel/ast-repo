# Apply the Context Cards pattern

You are a coding agent. Compress the knowledge of the project into small cards that load on demand, with an index in `CLAUDE.md`. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Follow Steps 0–5 of the procedure below.
2. Create the `cards/` directory and write the cards.
3. Append the index to `CLAUDE.md`.
4. Keep the index of the project's own cards apart from the cards that `ast` installed:
   - `CARDS.md` holds the lines of installed cards between two `ast:cards` marker
     comments. Do not write between the markers, and do not copy those lines.
   - Keep the `@CARDS.md` line in `CLAUDE.md` if it is there.
   - Do not give an installed card a second index line.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/cards-procedure.md}}
