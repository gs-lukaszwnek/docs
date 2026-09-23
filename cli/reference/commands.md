---
url: https://developer-portal.gainsight.com/docs/cli/reference/commands.md
description: Every Developer Studio CLI command, its flags, and its exit behavior.
---

# Command Reference

Documents every `gsds` subcommand at version `1.7.1`. Every command exits `0` on success and `1` on any failure. Only `gsds preview` and `gsds connector test` require an active session.

## `gsds init`

Scaffolds a new project, or backfills missing files into an existing widget project. Writes only whatever's missing — `gsds.json`, `extensions_registry.json`, `connectors_registry.json`, `AGENTS.md`, `README.md`, and a `.gitignore` that excludes `.gsds/` — and never overwrites a file it finds. A `gsds.json` that predates shared-dependency deduplication (no `dedupe` key at all) gets `@angular/*` backfilled into its exclude list automatically; a `gsds.json` that already has a `dedupe` key, even an empty one, is left untouched.

| Argument | Required | Description |
|---|---|---|
| `<name>` | Yes | Directory to create or reuse. Pass `.` to scaffold the current directory in place — the project name falls back to the directory's own name |
| `--force` | No | Allow scaffolding into a non-empty directory that isn't recognized as an existing widget project (see below) |

### When you need `--force` — and when you don't

`gsds init` recognizes an existing widget project automatically, with no flag, when the target directory already has any of:

* `extensions_registry.json`
* `connectors_registry.json`
* `gsds.json`
* the legacy `widget_registry.json` — migrated to `extensions_registry.json` automatically, content preserved byte-for-byte

This covers the common case of an existing repo: one cloned or forked from the [Widgets Repository Template](https://github.com/gainsight-hub/widgets-repository-template), or any project already using `gsds`. Re-running `gsds init` against it backfills only what's missing (for example a `gsds.json` a template clone never had) and leaves every existing file untouched.

`--force` is only for a non-empty directory with **none** of the markers above — for example a freshly created GitHub repo with just a `README.md` or `LICENSE` from the "initialize this repository" checkbox. `--force` means "I know this directory is safe to scaffold into"; it does not relax the never-overwrite guarantee, and it is not the default answer to "I already have a repo" — most existing widget repos already carry one of the markers above and need no flag at all. A bare `widgets/` directory by itself does not count as a marker: it's too generic a signal (design assets, an unrelated monorepo subfolder) to prove a widget project on its own.

## `gsds login`

Redeems a pairing code, reports the current session, or does nothing when no arguments are provided. Each tenant you log into is kept as its own profile; the one you just logged into becomes current.

| Argument | Description |
|---|---|
| `<pairing_code>` | Redeem a pairing code from **Integrations → Developer Studio → CLI Access** in the community. Codes expire 1 minute after issue |
| `--status` | Print the current session's tenant binding and validity |
| `--profile <name>` | Store the resulting session under an explicit profile name instead of the tenant-derived default |

The resulting session lasts 8 hours and is tenant-scoped. There is no refresh flow.

## `gsds logout`

Clears the stored session for the current profile. No flags besides `--profile <name>`, which clears a specific profile's session instead of the current one.

## `gsds profile`

Lists stored login profiles and marks the current one.

| Flag | Description |
|---|---|
| `--json` | Emit the profile list as machine-readable JSON instead of a table |

### `gsds profile use <name>`

Switches which stored profile is current. Every subsequent command that needs a session (`preview`, `logout`, `connector test`) uses this profile automatically until you switch again.

## `gsds create`

Scaffolds a widget under `widgets/<name>/`. Interactive in a **TTY** (a terminal a person is typing into); in a non-TTY session (CI, an AI agent) the three flags below are required and the command fails fast when any is missing.

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
| `--profile <name>` | Preview using a specific stored profile instead of the current one |

Requires an active session. Each widget must declare a `dev` script in its `package.json` to appear in the picker. The package manager for each widget is auto-detected from that widget's lockfile.

## `gsds build`

Regenerates `extensions_registry.json` and `connectors_registry.json` from source. Installs each widget's dependencies automatically first (concurrently) if missing — no manual `npm install` needed per widget; a widget whose install fails is skipped without blocking the rest.

| Flag | Description |
|---|---|
| `--validate` | Compare registries against source and exit `1` on drift. Does not rebuild `dist/`. Intended as a CI gate |

Every `widget.json` must declare a non-empty `title` and `category`, or the build fails and names the offending file.

### Shared dependency deduplication

Any package declared in two or more widgets' `package.json` `dependencies` is externalized out of those widgets' bundles automatically, and the registry gets an `importMaps` entry so the browser loads one shared copy. `--validate` also checks `importMaps` for drift when every shared dependency's version can be resolved without installing anything; otherwise it skips that comparison rather than false-failing. See [Share Dependencies Across Widgets](../share-dependencies) for the full mechanics, exclusions, and version-conflict rules.

## `gsds connector test`

Runs a Connector against the paired tenant. Every run reports which of three resolution steps won: a local `widgets/*/connectors.json` entry, the root `connectors_registry.json`, or the connector persisted on the tenant.

| Argument | Description |
|---|---|
| `[name]` | Connector to run. Resolution is local-first, in order: `widgets/*/connectors.json`, then `connectors_registry.json`, then the tenant |
| `--payload @<file>` | Path to a JSON file containing the request body. File reference only — inline JSON is rejected |
| `--query k=v` | Query parameter. Repeatable |
| `--path-param k=v` | Path parameter. Repeatable |
| `--verbose` | Print the rendered upstream request |
| `--json` | Emit a machine-readable result |
| `--profile <name>` | Run against a specific stored profile instead of the current one |

Requires an active session. Composite Connectors cannot be tested.

When a name resolves from a per-widget file, `gsds` warns on stderr if `connectors_registry.json` disagrees — either it defines the connector differently (drift since the last `gsds build`) or it doesn't define it at all. Both point at running `gsds build`. An unbuilt registry (absent, or with no connector entries at all) stays silent.

Every run is captured under `.gsds/` with sensitive headers redacted.

## `gsds update`

Checks the npm registry for a newer CLI version now, and installs it after you confirm.

No flags. In a non-TTY shell (or with `CI=true`), it prints the `npm install -g` command instead of running it, and exits `0` without installing. This is also the only way to take a major-version or prerelease upgrade — `gsds`'s automatic background check (see [Automate with CI and AI](../automate-with-ci-and-ai#control-the-automatic-update)) only ever installs a newer version within the current major.

## `gsds script` and `gsds style`

Manage sitewide scripts and stylesheets. Both commands share the same subcommand shape.

### Subcommands

| Subcommand | Description |
|---|---|
| `new <slug>` | Scaffold a local file and register it |
| `link <url>` | Register an external hosted URL. Must end in `.js` for scripts, `.css` for stylesheets |
| `ls` | List registered entries |
| `rm <slug>` | Remove an entry |

### Flags on `new` and `link`

| Flag | Applies to | Description |
|---|---|---|
| `--attr k=v` | Both | HTML attribute on the injected tag. Repeatable |
| `--page <name>` | Both | Scope injection to a single page instead of sitewide |
| `--placement head\|bodyStart\|bodyEnd` | Scripts only | Where to inject the script tag |
| `--name <slug>` | `link` only | Explicit slug. When omitted, the slug is derived from the URL |

### Flags on `rm`

| Flag | Description |
|---|---|
| `--force` | Delete a folder that is not empty |

## Global flags

| Flag | Description |
|---|---|
| `--version` | Print the installed CLI version and exit |

## Related

* Session model and profiles → [Authenticate](../authenticate)
* Shared dependency deduplication → [Share Dependencies Across Widgets](../share-dependencies)
* On-disk files → [Project files](project-files)
* Error messages → [Troubleshooting](troubleshooting)
