# Apply the Hooks Over Prose pattern

You are a coding agent. Move mechanical "always/never" rules out of `CLAUDE.md` into `.claude/settings.json` permissions and hooks, so that the harness enforces them. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Scan every `CLAUDE.md` (and its imports) for rules with "always", "never", "before
   every" or "don't forget".
2. Show the mechanical ones to the user, each with the permission rule or hook that you
   propose. Apply only the ones that the user approves.
3. A rule can need a tool (a formatter, `jq`). Detect the tool after the user approves the
   rule, and follow `ast prompt install-tools`. If the user skips the tool, leave out
   that rule and keep the others.
4. Put shared rules in `.claude/settings.json`, merged with the keys that are there. Put
   hook scripts in `.claude/hooks/` and make them executable. Replace the prose of each
   moved rule with a one-line note.
5. Trigger each new rule once. Confirm that it fires with the message you expect.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/hooks-over-prose.md}}
