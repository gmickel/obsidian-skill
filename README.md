# obsidian-skill

Agent skill for the [Obsidian CLI](https://help.obsidian.md/cli) (1.12+). Read, create, search, and organize notes in Obsidian vaults. Covers daily notes, tasks, tags, properties, templates, bookmarks, plugins, knowledge graph analysis, and developer tools.

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
[![Twitter](https://img.shields.io/badge/twitter-@gmickel-1DA1F2.svg)](https://twitter.com/gmickel)

## Install

```bash
npx skills add gmickel/obsidian-skill
```

### Prerequisites

1. [Obsidian](https://obsidian.md) 1.12+
2. Enable CLI: **Settings > General > Command line interface** > follow registration prompt
3. Restart the terminal. Depending on platform and installer, the registered
   executable may be `obsidian` or `obsidian-cli`; the skill probes candidates
   with the official `version` command before use.

## What's included

| File | Covers |
|------|--------|
| `SKILL.md` | Capability probe, vault targeting, safety rules, core operations, reference navigation |
| `references/note-operations.md` | Read, create, append, prepend, move, delete, outline, word count, unique notes |
| `references/search-metadata.md` | Search, tags, properties, aliases, backlinks, orphans, unresolved links |
| `references/daily-tasks-templates.md` | Daily notes, tasks, templates |
| `references/vault-management.md` | Vault info, files/folders, bookmarks, plugins, workspaces, themes, sync, publish, bases, dev tools |

The skill uses progressive disclosure: agents load the core workflow on activation and pull in reference files on demand. It contains no personal vault names or machine-specific user paths.

## Compatibility

- macOS, Linux, Windows
- Works with any AI agent that supports the [Agent Skills](https://agentskills.io) format (Claude Code, Codex, OpenCode, etc.)

## Links

- [Obsidian CLI docs](https://help.obsidian.md/cli)
- [Agent Skills spec](https://agentskills.io)

## License

[MIT](./LICENSE)

---

<p align="center">
  made with ❤️ by <a href="https://twitter.com/gmickel">@gmickel</a>
</p>
