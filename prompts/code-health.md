# Apply the Code Health Feedback pattern (experimental)

You are a coding agent. Use or install the aihealth code-health tool, pinned to a release, and wire it into Claude Code: hooks that report new or worse smells after each edit, and an MCP server for checks on demand. Work in the project in the current directory.
Do the steps in order. Do not make a commit.

## Steps

1. Follow the steps "Applying it to a project" in the pattern below. The file
   `AGENT-INSTALL.md` of the aihealth repo, section "In a project's venv", holds the exact
   commands and JSON.
2. Get aihealth and a Python 3.11+ interpreter through `ast prompt install-tools`. Use a
   suitable copy that exists, or install where the user chooses. The default is the
   project's gitignored `.venv/`. Install outside the project only if the user picks that.
3. Pin the install to the commit of the latest release tag. The pattern shows how to find
   that commit. Someone can move a tag, but nobody can move a commit. Commit the pin: in
   the dev requirements, or as the install command in `CLAUDE.md`.
4. Wire `.venv/bin/aihealth` into `.mcp.json` and the hooks in `.claude/settings.json`.
   Merge with the entries that are there, and keep the `[ -x … ]` guard. Add the short
   `CLAUDE.md` section.
5. Run the baseline scan and report it. Do not refactor what it finds unless the user asks.
6. If the user skips aihealth, make no file changes for this pattern and say so.

## Check

Run `python3 -I .claude/scripts/check-agent-docs.py .` if that script is installed. Fix each
ERROR that this pattern caused. Report the others. Judge each WARN.

---

The pattern, in full:

{{include cards/code-health.md}}
