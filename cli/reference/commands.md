---
url: https://developer-portal.gainsight.com/docs/cli/reference/commands.md
description: Every Developer Studio CLI command, its flags, and its exit behavior.
---

# Command Reference

Documents every `gsds` subcommand at version `1.8.2`. Every command exits `0` on success and `1` on any failure, and `130` when you interrupt it with `Ctrl-C`. Only `gsds preview` and `gsds connector test` require an active session.

## `gsds init`

Scaffolds a new project, or backfills missing files into an existing widget project. Writes only whatever's missing — `gsds.json`, `extensions_registry.json`, `connectors_registry.json`, `AGENTS.md`, `README.md`, and a `.gitignore` that excludes `.gsds/` — and never overwrites a file it finds. A `gsds.json` that predates shared-dependency deduplication (no `dedupe` key at all) gets `@angular/*` backfilled into its exclude list automatically; a `gsds.json` that already has a `dedupe` key, even an empty one, is left untouched. It also refreshes `gsds.json`'s `cliVersion` to the version of `gsds` you are running, reporting `Updated gsds.json cliVersion 1.5.0 -> 1.8.0.` — this is what clears the stale-version notice every other command prints while that field is behind.

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

A `--profile` value is trimmed of surrounding whitespace and capped at 96 characters. An empty or whitespace-only value is rejected before the pairing code is redeemed, so a mistyped flag does not burn the single-use code.

## `gsds logout`

Clears the stored session for the current profile. No flags besides `--profile <name>`, which clears a specific profile's session instead of the current one.

When other profiles remain, `gsds logout` promotes one to current and prints `<name> is now the current profile.` It prefers a profile that is neither expired nor unreadable. When only an unusable one is left, it says so on its own line:

```
That session needs a new login - run `gsds login <pairing_code>`.
```

## `gsds profile`

Lists stored login profiles, marks the current one, and marks any that need a new login.

Each row is the profile name followed, in parentheses, by whatever applies: `current`, `expired` when its stored session has timed out, and `unreadable` when its stored credential cannot be read. An expired profile stays stored and stays switchable — it is the name to log back in under.

```
acme-en (current)
acme-eu (expired)
acme-sandbox (unreadable)
```

| Flag | Description |
|---|---|
| `--json` | Emit the profile list as machine-readable JSON instead of a table. Each row carries `profile`, `tenant`, `current`, `expired`, and `unreadable` |

### `gsds profile use <name>`

Switches which stored profile is current. Every subsequent command that needs a session (`preview`, `logout`, `connector test`) uses this profile automatically until you switch again. The name is matched case-insensitively and ignores surrounding whitespace.

## `gsds create`

Scaffolds a widget under `widgets/<name>/`. Interactive in a **TTY** (a terminal a person is typing into); in a non-TTY session (CI, an AI agent) the three flags below are required and the command fails fast when any is missing.

| Flag | Required (non-TTY) | Description |
|---|---|---|
| `--name` | Yes | Widget slug |
| `--framework` | Yes | One of `react`, `vue`, `angular`, `vanilla`, `html`. Case-insensitive |
| `--category` | Yes | Category label used in the No-Code Builder |

`gsds create` writes no `widget.json`. The new widget exists only as an entry appended to `extensions_registry.json`, in the same shape a hand-authored widget has. Appending holds a lock on the registry file while it writes, so parallel `gsds create` runs cannot drop each other's entries.

It then runs an incremental build so that a dependency the new widget now shares with another widget is pulled out of both bundles straight away — only the new widget, plus any widget whose shared dependencies actually changed, not every widget in the project. See [Share Dependencies Across Widgets](../share-dependencies).

## `gsds list`

Lists Extensions in the current project.

| Flag | Description |
|---|---|
| `--json` | Emit a keyed object `{ widgets, connectors, scripts, stylesheets }` instead of a table. Not a bare array. The stale-`cliVersion` notice goes to stderr, so stdout stays valid JSON |

## `gsds preview`

Boots a local dev server, registers a preview session with the paired community, and makes local widgets appear in the No-Code Builder picker.

| Flag | Description |
|---|---|
| `--port` | Override the preview port. Default `5173` |
| `--widget <name>` | Restrict the preview to specific widgets. Repeatable |
| `--profile <name>` | Preview using a specific stored profile instead of the current one |

Requires an active session, paired under the same account you sign in to your community with — the No-Code Builder looks for a preview session belonging to the account you are signed in as.

A widget appears in the picker if it declares a `dev` script in its `package.json`, or — with no `dev` script — its registry entry declares a `source`, in which case the preview server serves it directly. A widget with neither is skipped, and `gsds preview` names it. For a widget that does have a `dev` script, the package manager is resolved from that widget's `package.json` `packageManager` field first, and from its lockfile when that field is absent.

For a widget served as static HTML, asset references resolve the way publishing resolves them: a root-absolute reference such as `/app.js` resolves against the widget root, and a reference naming no file in the widget is left exactly as written, with a one-time warning so a typo stays visible.

Preview serves from `http://localhost:<port>`, and each JavaScript widget's own `dev` script serves from a further port of its own — HTML-only widgets add none. Your community page loads from all of them directly, which needs a browser permission the first time. See [Troubleshooting](troubleshooting#the-browser-blocks-the-local-preview).

## `gsds build`

Builds each widget, then validates and normalizes `extensions_registry.json` and `connectors_registry.json` in place.

Both registries are **hand-authored sources of truth**, not generated output. `gsds build` never derives widget entries from a scan of `widgets/`, and a widget with no registry entry is invisible in your community. Field order, array order, and top-level keys `gsds` does not manage are all preserved, so a build with nothing to change produces no diff.

A widget's `type` is the identifier the platform uses to match a widget already placed on a community page to its source. `gsds build` leaves it exactly as the registry declares it, and never derives it from the folder name.

Normalizing writes only the parts `gsds` manages: the `importMaps` entries for shared dependencies, and dropping a `scripts` or `stylesheets` key once its last entry is removed. Everything you wrote is left as you wrote it.

Installs each widget's dependencies automatically first (concurrently) if missing — no manual `npm install` needed per widget. A widget whose install fails is left out of the rebuild pass, and the build then fails, naming every widget it could not prepare.

Every widget entry in `extensions_registry.json` must declare a non-empty `title` and `category`, or the build fails and names the offending entry.

| Flag | Description |
|---|---|
| `--validate` | Check that every registry entry still resolves to the files it names, and exit `1` on drift. Does not rebuild `dist/`. Intended as a CI gate |

Registry writes take the same lock, one writer at a time, so parallel `gsds build`, `gsds create`, and `gsds script new` runs cannot drop each other's entries.

### Files `gsds build` ignores

| What `gsds build` finds | What it means |
|---|---|
| `widget.json` or `connectors.json` inside a widget that **is** in `extensions_registry.json` | Redundant — the widget is already declared in the registry. Safe to delete |
| `widget.json` inside a widget that is **not** in `extensions_registry.json` | That file is the widget's only remaining metadata. Migrate it into the registry — deleting it loses the widget |
| A `widgets/<name>/` folder with no registry entry | The platform will never see it. Add an entry, or delete the folder if it is not a widget |

### Missing registries

A registry file absent from a project that already had one is refused, not rebuilt from scratch: the file is the source of truth, so rebuilding cannot recover what it held. Restore it from version control. See [Troubleshooting](troubleshooting#a-registry-file-is-missing).

### What else `gsds build` refuses

Each of these fails the build and names what it found.

| Condition | What it means |
|---|---|
| Two widget entries declaring the same `type` | The `type` no longer identifies one widget. A duplicate Connector permalink fails the same way |
| An entry whose `source` block is incomplete, or whose `source.path` points outside its own `widgets/<name>/` folder | The entry does not resolve to files belonging to that widget |
| A widget whose `package.json` is not valid JSON | The widget's build step cannot be read |
| A `gsds.json` that is not valid JSON, or not the expected shape | `dedupe.exclude` cannot be read, so which packages are excluded from deduplication is unknown |
| A registry file that is a symlink, or that is read-only | Writing publishes through a rename, which replaces the path itself: a symlink would be deleted rather than followed, and a read-only file would be replaced regardless of the permission set on it |

### Shared dependency deduplication

Any package declared in two or more widgets' `package.json` `dependencies` is externalized out of those widgets' bundles automatically, and the registry gets an `importMaps` entry so the browser loads one shared copy. `--validate` also checks `importMaps` for drift when every shared dependency's version can be resolved without installing anything; otherwise it skips that comparison rather than false-failing. See [Share Dependencies Across Widgets](../share-dependencies) for the full mechanics, exclusions, and version-conflict rules.

## `gsds connector test`

Runs a Connector against the paired tenant. Resolution is local-first in two steps — `connectors_registry.json`, the root file the platform ingests, then the Connector persisted on the tenant — and every run reports which of the two won.

| Argument | Description |
|---|---|
| `[name]` | Connector to run. Resolution is local-first, in order: `connectors_registry.json`, then the tenant |
| `--payload @<file>` | Path to a JSON file containing the request body. File reference only — inline JSON is rejected |
| `--query k=v` | Query parameter. Repeatable |
| `--path-param k=v` | Path parameter. Repeatable |
| `--verbose` | Print the rendered upstream request |
| `--json` | Emit a machine-readable result |
| `--profile <name>` | Run against a specific stored profile instead of the current one |

Requires an active session. Composite Connectors cannot be tested.

The `source:` line prints before the request goes out. A local match states a fact; the tenant step can only state an intent, and reads `source: no local definition - trying a connector persisted on your tenant`. When neither holds the name, the failure reads `Connector "x" is not defined in this project, and your tenant has no persisted connector under that name.`

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
| `link <url>` | Register an external hosted URL. Must end in `.js` for scripts, `.css` for stylesheets — the extension is matched case-insensitively |
| `ls` | List registered entries |
| `rm <slug>` | Remove an entry |

### Flags on `new` and `link`

| Flag | Applies to | Description |
|---|---|---|
| `--attr k=v` | Both | HTML attribute on the injected tag. Repeatable. Pass a bare `--attr defer` for a valueless attribute; an empty name (`--attr =v`) is rejected |
| `--page <name>` | Both | Scope injection to a single page instead of sitewide. Repeatable; identical values collapse to one rule |
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

`gsds` with no arguments prints help on stdout and exits `0`. `gsds help <command> <subcommand>` resolves to any depth, so `gsds help script new` shows the help for `new` rather than for `script`.

## Related

* Session model and profiles → [Authenticate](../authenticate)
* Shared dependency deduplication → [Share Dependencies Across Widgets](../share-dependencies)
* On-disk files → [Project files](project-files)
* Error messages → [Troubleshooting](troubleshooting)
