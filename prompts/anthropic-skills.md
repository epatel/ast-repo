# Enable Anthropic's skills for this project

You are a coding agent. Offer the plugins of Anthropic's official skills marketplace
(`https://github.com/anthropics/skills`, marketplace name `anthropic-agent-skills`) that
match the project in the current directory. `ast` does not install plugins: this prompt
changes `.claude/settings.json`. Do the steps in order. Do not make a commit.

1. Read `.claude-plugin/marketplace.json` in that repo for the current list of plugins.
   Do not rely on a list from memory. At the time of writing: `document-skills` (docx,
   pdf, pptx, xlsx), `example-skills` (frontend-design, mcp-builder, webapp-testing,
   skill-creator, doc-coauthoring, canvas-design and more), `claude-api`.
2. Recommend a plugin only when the work of the project matches it:
   - a web frontend → `example-skills`, for `frontend-design` and `webapp-testing`
   - office document generation → `document-skills`
   - work on the Claude API → `claude-api`
3. Check `/plugin` and the skills listed in the session before you propose a plugin.
   - Skip a plugin whose skills the user already has from another marketplace or built
     into their Claude account.
   - If the user has the same plugin at user scope, still offer it at project scope, so
     that collaborators get it. Say that the user can then remove the user-scope install.
4. After the user approves, enable each plugin for the whole project in
   `.claude/settings.json`. Merge with the keys that are there:
   ```json
   {
     "extraKnownMarketplaces": {
       "anthropic-agent-skills": { "source": { "source": "github", "repo": "anthropics/skills" } }
     },
     "enabledPlugins": { "example-skills@anthropic-agent-skills": true }
   }
   ```
5. If `/plugin` reports that the plugin "is enabled in project settings but isn't
   installed", run `claude plugin install <plugin>@anthropic-agent-skills --scope project`.
   Never run `/plugin install` or `claude plugin install` without `--scope project`: the
   default is user scope.
6. Tell the user: collaborators get the marketplace after they accept the workspace trust
   dialog, and each plugin skill has the plugin's name as a namespace
   (`/example-skills:frontend-design`).

Each skill has its own `LICENSE.txt`. Most are Apache 2.0, but the document skills are
"All rights reserved". Use the document skills, but do not copy their files into the project.
