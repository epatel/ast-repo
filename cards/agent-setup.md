# agent-setup

How this project's agent tools (skills, commands, scripts, cards) are installed and kept current with `ast`.

The files under `.claude/skills/`, `.claude/commands/`, `.claude/scripts/` and some files in
`cards/` are copies. They come from the organisation's setup repo and from the skill repos
it references. Do not edit a copy in place: the next update reports it as a local change
and stops.

## The files

- `ast.toml` names the org repo, the selected set, and the tools this project adds or
  turns off. Edit it by hand or with the commands below.
- `ast.lock` records the exact commit and content of each installed tool. Do not edit it.
- Both files and all installed copies are committed with the project.

## Commands

| To do this | Run |
| --- | --- |
| See what is installed and whether anything changed | `ast status` |
| See what else the org offers | `ast list --available` |
| Take the newest version of the set and its tools | `ast update` |
| Turn one of the set's tools off in this project | `ast disable <tool>` |
| Add a tool that the set does not have | `ast add <tool>` |
| Change to another set | `ast select <set>` |
| Put back a copy that was deleted or damaged | `ast sync --force` |

Run `ast update` on its own branch and review the diff of the installed files before you
merge it. An update can change the instructions that every agent in this project reads.

## When a tool needs a change

1. Change it in the repo it comes from. `ast status` shows the source of each tool.
2. Release it there (a tag, when the source is followed with `latest-tag`).
3. Run `ast update` here.

To change a tool for this project only, turn it off with `ast disable <tool>` and keep your
own version under a different name.

## The card index

`ast` lists the installed cards in `CARDS.md`, between the two `ast:cards` marker comments,
and `CLAUDE.md` imports that file with `@CARDS.md`. Do not edit between the markers: `ast`
rewrites that part on every sync. To index a card of your own, add its line outside the
markers. To change the text of an installed card's line, change it in the `CARDS.md` of
the repo the card comes from.
