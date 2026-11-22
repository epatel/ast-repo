# Install a tool that a pattern or skill needs

You are a coding agent. A pattern or skill needs a tool: an interpreter, a CLI, a package.
The pattern lists it in a `## Requires` table (tool, minimum version, how to detect it,
default location). Every tool goes through the same steps: detect, check the version, ask
whether and where, install, verify. Never install anything without an explicit yes from
the user. $ARGUMENTS

1. **Detect, in this order.** Stop at the first copy that works:
   1. The project's own location: `.venv/bin/<tool>`, `node_modules/.bin/<tool>`.
   2. `command -v <tool>` on `PATH`.
   3. For a Python interpreter, versioned names and managers: `python3.13`, `python3.12`,
      `python3.11`, `uv python find '>=3.11'`.

   Take the version from the tool's own `--version` output. Compare versions number by
   number (`3.9` is older than `3.11`), never as strings. For an interpreter, report each
   one that qualifies with its path, and recommend the newest.
2. **Check that it works, not only that it exists.** For Python, also create a throwaway
   venv: `python3 -m venv "$(mktemp -d)/t"`. On Debian and Ubuntu this fails with
   `ensurepip is not available` until the user installs `python3-venv`.
3. **Ask once, after the user confirms the scope.** Put one question for each missing or
   outdated tool in a single message. Each question gives the tool, the pattern that needs
   it, the version found (or "not found"), the minimum version, and the location options,
   with the recommended one first:

   | Tool type | Options, recommended first |
   |---|---|
   | Python tool for this project (e.g. aihealth) | Suitable copy that exists · project `.venv/` · separate project tool venv `.venv-tools/` (when its dependencies clash with the project's) · user level with `uv tool install` or `pipx` (all projects) · skip |
   | Python interpreter | `python3.1x` that exists, or a uv-managed interpreter · `uv python install <version>` (user level) · Homebrew or apt (system-wide) · skip the pattern |
   | `python3-venv` on Debian/Ubuntu | The user runs `sudo apt install python3-venv` · use `uv venv` instead · skip |
   | CLI tool used by a hook (`jq`, a formatter) | Project-local (`.venv/` for Python tools, `devDependencies` for Node tools) · Homebrew or apt (system-wide) · skip, and the hook does nothing |
   | `git`, `curl`, `make`, `claude`, `ast` | Never installed by this procedure. If missing or too old, stop and tell the user |

   - Mark each option outside the project as *affects all projects* or *system-wide*.
   - When a suitable copy exists, even outside the recommended location, offer "use the
     copy at `<path>`" first. Say what this means for collaborators: they need the same form.
4. **Install only what the user approved, where the user approved it.**
   - Show the exact command before you run it.
   - Never run `sudo` yourself. Give the user the command to run.
   - After the install, detect the tool again and confirm that the version meets the minimum.
   - If an install fails, report the error and stop. Do not change to another method
     without asking.
5. **If the user skips a tool, what needs it is declined.**
   - A tool that the whole pattern needs: make no file changes for that pattern.
   - A tool that only one rule or hook needs: leave out that rule and keep the others.

   Say in one line what you left out, and that the user can ask for it later.
6. **Write the install command where the pattern says**: a requirements file, the
   `CLAUDE.md` section, a `make setup` target. Never write machine-specific paths or the
   version found on this machine. Collaborators run the same steps on their own machines.
