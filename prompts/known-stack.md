# Apply the Known Stack pattern (experimental)

You are a coding agent. Name the standard frameworks and patterns of the project in `CLAUDE.md`, one line each, so that cards cover only what the code and your own knowledge cannot supply. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Apply this pattern before Context Cards.
2. Write the **Known stack** section in `CLAUDE.md`, between the overview and the commands.
3. Write one line for each standard concept, with its file paths and any local twist.
   A concept that needs more than one line becomes a card.
4. Then run a narrowed cards procedure (`ast prompt context-cards`). Write no architecture
   card if the architecture is a textbook instance of the named stack. Write no domain
   cards for layers that the lines cover.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/known-stack.md}}
