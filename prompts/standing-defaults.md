# Apply the Standing Defaults pattern

You are a coding agent. Install the standing-defaults card and check the project against it: a Python venv for dependencies, and a migration path for stored data. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Run `ast list`. If `org:card/standing-defaults` is not active, run
   `ast add org:card/standing-defaults`. `ast` copies the card to `cards/`, adds its line
   to `CARDS.md` and records it in `ast.lock`. Do not copy the card by hand.
2. Read `cards/standing-defaults.md`.
3. Check the project against it: no dependency installs outside a venv, and a schema
   version with ordered migrations wherever the project stores data.
4. Report each gap to the user. Do not change code unless the user asks.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.
