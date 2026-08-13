---
name: obsidian-skill
description: Use Obsidian's official CLI to work with a running Obsidian app and its configured vaults. Use for live workspace state, indexed search and metadata, daily notes, tasks, links and backlinks, Bases, templates, bookmarks, plugins, themes, Sync status, and developer inspection. Prefer ordinary filesystem tools when the task only needs direct file reads, writes, listing, or text search.
---

# Obsidian CLI

Use the CLI for behavior that depends on Obsidian's running application,
indexes, settings, command registry, or developer runtime. Use normal filesystem
tools for ordinary file operations.

## Resolve and probe the CLI

Do not assume one executable name. Obsidian registration and packaging differ
by platform and release, and unrelated third-party binaries may use the same
name. Select a candidate only when its `version` probe succeeds.

On macOS/Linux:

```bash
resolve_obsidian_cli() {
  local candidate
  for candidate in \
    "${COPILOT_OBSIDIAN_CLI:-}" \
    /Applications/Obsidian.app/Contents/MacOS/obsidian-cli \
    "$(command -v obsidian 2>/dev/null || true)" \
    "$(command -v obsidian-cli 2>/dev/null || true)"
  do
    [[ -n "$candidate" && -x "$candidate" ]] || continue
    if "$candidate" version >/dev/null 2>&1; then
      printf '%s\n' "$candidate"
      return 0
    fi
  done
  return 1
}

OBSIDIAN_CLI=$(resolve_obsidian_cli) || {
  echo "Obsidian CLI unavailable" >&2
  exit 1
}
"$OBSIDIAN_CLI" version
```

On PowerShell:

```powershell
$candidates = @(
  $env:COPILOT_OBSIDIAN_CLI,
  (Get-Command obsidian -ErrorAction SilentlyContinue).Source,
  (Get-Command obsidian-cli -ErrorAction SilentlyContinue).Source
) | Where-Object { $_ }

$OBSIDIAN_CLI = $candidates | Where-Object {
  try { & $_ version *> $null; $LASTEXITCODE -eq 0 } catch { $false }
} | Select-Object -First 1

if (-not $OBSIDIAN_CLI) { throw "Obsidian CLI unavailable" }
& $OBSIDIAN_CLI version
```

If the probe fails, ask the user to open a compatible Obsidian release, enable
**Settings > General > Command line interface**, complete its registration, and
restart the terminal. Do not install Obsidian, change PATH, or register the CLI
without explicit user intent.

Use the resolved executable in place of `obsidian` in all reference examples:

```bash
"$OBSIDIAN_CLI" vaults
"$OBSIDIAN_CLI" help search
```

## Target the vault precisely

Use `vault="<name-or-id>"` whenever the target is known. Never embed a personal
vault name or machine path in this skill.

```bash
"$OBSIDIAN_CLI" vault="<vault-name>" search query="project deadline" format=json
"$OBSIDIAN_CLI" vault="<vault-name>" backlinks path="Projects/Plan.md" format=json
```

Use `path=` for an exact vault-relative path. Use `file=` only when Obsidian's
wikilink-style name resolution is wanted. Quote values containing spaces or
shell-special characters. Inspect live help before relying on syntax not shown
by the installed version:

```bash
"$OBSIDIAN_CLI" help <command>
```

## Core operations

```bash
# Read and write
"$OBSIDIAN_CLI" vault="<vault-name>" read path="Projects/Roadmap.md"
"$OBSIDIAN_CLI" vault="<vault-name>" append path="Journal.md" content="- New entry"

# Search and metadata
"$OBSIDIAN_CLI" vault="<vault-name>" search query="API" limit=10 format=json
"$OBSIDIAN_CLI" vault="<vault-name>" properties path="Projects/Roadmap.md"

# Configured daily note and indexed tasks
"$OBSIDIAN_CLI" vault="<vault-name>" daily:append content="- 2pm: Call" silent
"$OBSIDIAN_CLI" vault="<vault-name>" tasks all todo

# Live workspace
"$OBSIDIAN_CLI" vault="<vault-name>" tabs ids
"$OBSIDIAN_CLI" vault="<vault-name>" workspace ids
```

## Preserve the host session

Never reload or restart the Obsidian app/window from an agent session. Never
reload, disable, or uninstall the plugin hosting the current agent session.
Avoid equivalent command, JavaScript, or developer-tool actions. These can
terminate the session and discard in-flight work.

Require explicit user intent before permanent deletion, local-history or Sync
restoration, publishing or unpublishing, plugin/theme installation or removal,
restricted-mode changes, or mutating JavaScript/developer calls. Prefer
reversible operations. The command references describe capabilities; they do
not authorize mutations.

## Reference guides

Read only the guide relevant to the task. Treat `obsidian` in their examples as
the resolved `$OBSIDIAN_CLI` executable and add `vault="<name-or-id>"` when the
target is known.

| Task | Reference |
|------|-----------|
| Note operations, outline, word count, unique notes | [references/note-operations.md](references/note-operations.md) |
| Search, tags, properties, backlinks, orphans, unresolved links | [references/search-metadata.md](references/search-metadata.md) |
| Daily notes, tasks, templates | [references/daily-tasks-templates.md](references/daily-tasks-templates.md) |
| Vaults, workspace, bookmarks, plugins, themes, Sync, Publish, Bases, dev tools | [references/vault-management.md](references/vault-management.md) |
