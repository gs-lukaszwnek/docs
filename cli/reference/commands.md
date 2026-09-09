---
url: https://developer-portal.gainsight.com/docs/cli/reference/commands.md
description: Every Developer Studio CLI command, its flags, and its exit behavior.
---

# Command Reference

Documents every `gsds` subcommand at version `1.2.0`. Every command exits `0` on success and `1` on any failure. Only `gsds preview` and `gsds connector test` require an active session.

## `gsds init`

Scaffolds a new project. Writes `gsds.json`, empty `extensions_registry.json` and `connectors_registry.json`, `AGENTS.md`, `README.md`, and a `.gitignore` that excludes `.gsds/`.

| Argument | Required | Description |
|---|---|---|
| `<name>` | Yes | Directory to create |
| `--force` | No | Allow initializing into a non-empty directory |

## `gsds login`

Redeems a pairing code, reports the current session, or does nothing when no arguments are provided.

| Argument | Description |
|---|---|
| `<pairing_code>` | Redeem a pairing code from the community's **Connect CLI** flow. Codes expire 2 minutes after issue |
| `--status` | Print the current session's tenant binding and validity |

The resulting session lasts approximately 8 hours and is tenant-scoped. There is no refresh flow.

## `gsds logout`

Clears the stored session. No flags.

## `gsds create`

Scaffolds a widget under `widgets/<name>/`. Interactive in a TTY; in a non-TTY the three flags below are required and the command fails fast when any is missing.

| Flag | Required (non-TTY) | Description |
|---|---|---|
| `--name` | Yes | Widget slug |
| `--framework` | Yes | One of `react`, `vue`, `angular`, `vanilla`, `html` |
| `--category` | Yes | Category label used in the No-Code Builder |

## `gsds list`

Lists Extensions in the current project.

| Flag | Description |
|---|---|
| `--json` | Emit a keyed object `{ widgets, connectors, scripts, stylesheets }` instead of a table. Not a bare array |

## `gsds preview`

Boots a local dev server, registers a preview session with the paired community, and makes local widgets appear in the No-Code Builder picker.

| Flag | Description |
|---|---|
| `--port` | Override the default preview port |
| `--widget <name>` | Restrict the preview to specific widgets. Repeatable |

Requires an active session. Each widget must declare a `dev` script in its `package.json` to appear in the picker. The package manager for each widget is auto-detected from that widget's lockfile.

## `gsds build`

Regenerates `extensions_registry.json` and `connectors_registry.json` from source.

| Flag | Description |
|---|---|
| `--validate` | Compare registries against source and exit `1` on drift. Does not rebuild `dist/`. Intended as a CI gate |

Every `widget.json` must declare a non-empty `title` and `category`, or the build fails and names the offending file.

## `gsds connector test`

Runs a Connector against the paired tenant. Every run reports whether it was resolved locally (`source: local`) or from the tenant (`source: remote`).

| Argument | Description |
|---|---|
| `[name]` | Connector to run. Resolution is local-first: matches under `widgets/*/connectors.json` before matches on the tenant |
| `--payload @<file>` | Path to a JSON file containing the request body. File reference only — inline JSON is rejected |
| `--query k=v` | Query parameter. Repeatable |
| `--path-param k=v` | Path parameter. Repeatable |
| `--verbose` | Print the rendered upstream request |
| `--json` | Emit a machine-readable result |

Requires an active session when the Connector is not found locally. Composite Connectors cannot be tested.

Every run is captured under `.gsds/` with sensitive headers redacted.

## `gsds script` and `gsds style`

Manage sitewide scripts and stylesheets. Both commands share the same subcommand shape.

### Subcommands

| Subcommand | Description |
|---|---|
| `new <slug>` | Scaffold a local file and register it |
| `link <slug>` | Register an external hosted URL. URL must end in `.js` for scripts, `.css` for stylesheets |
| `ls` | List registered entries |
| `rm <slug>` | Remove an entry |

### Flags on `new` and `link`

| Flag | Applies to | Description |
|---|---|---|
| `--attr k=v` | Both | HTML attribute on the injected tag. Repeatable |
| `--page <name>` | Both | Scope injection to a single page instead of sitewide |
| `--placement head\|bodyStart\|bodyEnd` | Scripts only | Where to inject the script tag |
| `--name <slug>` | `link` only | Explicit slug |

### Flags on `rm`

| Flag | Description |
|---|---|
| `--force` | Delete a folder that is not empty |

## Global flags

| Flag | Description |
|---|---|
| `--version` | Print the installed CLI version and exit |

## Related

* Session model → [Authenticate](../authenticate)
* On-disk files → [Project files](project-files)
* Error messages → [Troubleshooting](troubleshooting)
