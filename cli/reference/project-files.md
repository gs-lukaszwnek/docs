---
url: https://developer-portal.gainsight.com/docs/cli/reference/project-files.md
description: >-
  Files the Developer Studio CLI creates, reads, and writes — in your project,
  on your user account, and via environment variables.
---

# Project Files

Describes every path `gsds` touches. Commit rules are stated per file.

## In your project

### Committed

| Path | Purpose |
|---|---|
| `gsds.json` | Project-root marker. `gsds` refuses to run outside a directory containing this file, apart from `gsds init`. Its `cliVersion` and `templateVersion` record which CLI and which template shape the project was scaffolded with; `gsds init` refreshes `cliVersion`, which is what clears the stale-version notice. Its `dedupe.exclude` array names packages or scopes (a trailing `/*`) that `gsds build` never externalizes — see [Share Dependencies Across Widgets](../share-dependencies) |
| `extensions_registry.json` | Hand-authored registry of widgets, scripts, and stylesheets in the project. Never regenerated from a folder scan — `gsds build` validates it and writes only the parts it manages, as described under [`gsds build`](commands#gsds-build) |
| `connectors_registry.json` | Hand-authored registry of Connectors and composite Connectors. Validated and left as written on the same terms |
| `AGENTS.md` | AI-agent-oriented project notes seeded by `gsds init` |
| `README.md` | Human-oriented project notes seeded by `gsds init` |
| `.gitignore` | Adds `.gsds/` to the ignore list |

### Not committed

| Path | Purpose |
|---|---|
| `.gsds/` | Connector-test capture output, with sensitive headers redacted. `gsds init` gitignores this directory |
| `*.lock` | Transient. A lock is held next to a registry while a write is in progress, so parallel `gsds` runs cannot drop each other's entries. Removed when the write completes |
| `*.tmp` | Transient. Staging files written next to their destination and renamed into place, so an interrupted write cannot leave a half-written registry. Removed on every failure path |

`gsds init` adds `*.lock` and `*.tmp` to the `.gitignore` it writes for a new project. A project that already has a `.gitignore` keeps it exactly as found — `gsds init` never overwrites or merges into a file it finds, and re-running it changes nothing here — so an older project needs both patterns added by hand.

### Ignored by `gsds build`

| Path | Status |
|---|---|
| `widgets/<name>/widget.json` | Never read. Declare the widget in `extensions_registry.json` instead |
| `widgets/<name>/connectors.json` | Never read. Declare Connectors in `connectors_registry.json` instead |

`gsds build` warns when it finds either file, and when it finds a `widgets/<name>/` folder with no registry entry at all. Dot-directories and `node_modules` under `widgets/` are skipped entirely, so editor caches and installed packages raise no warning. See [`gsds build`](commands#gsds-build) for what to do in each case.

Widget entries live in `extensions_registry.json`, and must declare a non-empty `title` and `category` and exactly one of `source` or `content`. Connector permalinks must be unique. Scripts and stylesheets have no separate source manifest either — their entries live only in `extensions_registry.json` and are edited through `gsds script` and `gsds style`.

### Written by `gsds build`

| Path | What changes |
|---|---|
| `widgets/<name>/vite.config.ts` | The `externalPackages` array is patched in place when the widget's set of shared dependencies changes. Nothing else in the file is touched |
| `widgets/<name>/angular.json` | `architect.build.options.externalDependencies` is patched the same way, for Angular widgets |

Both are part of the shared-dependency deduplication mechanism — see [Share Dependencies Across Widgets](../share-dependencies). Commit the result; don't hand-edit either value.

## In your user account

### OS keychain

`gsds` stores the session token in your operating system's keychain:

| Platform | Backend |
|---|---|
| macOS | Keychain |
| Windows | Credential Store |
| Linux | Secret Service or keyutils via `@napi-rs/keyring` |

Service name: `gainsight-developer-studio-cli`. One account per profile — `default` unless you passed `--profile <name>` to `gsds login`.

### File fallback

When the keychain is unavailable — no supported provider, headless CI, or a keychain error — `gsds` writes to `~/.config/gsds/credentials.json` (`credentials.<profile>.json` for a named profile) at file mode `0600` and prints a one-time warning on stderr.

`gsds` also checks that mode every time it reads the file, and tightens it back to `0600` with a one-time warning if it finds the file more permissive — a restored backup or a loose `umask` cannot leave your token readable without telling you.

### Other files under `~/.config/gsds/`

| Path | Purpose |
|---|---|
| `current-profile` | Tracks which stored profile is current |
| `update-check.json` | Cache for the automatic update check: last-checked timestamp, last outcome, and last version installed. Read before deciding whether the 8-hour check window has elapsed |

## Environment variables

| Variable | Effect |
|---|---|
| `GSDS_DISABLE_KEYCHAIN=1` | Force the file-based credential fallback even when a keychain is available |
| `GSDS_DISABLE_AUTO_UPDATE=1` | Skip the automatic update check entirely — no registry call, no install, no cache write |

## Related

* Session pairing, TTL, and profiles → [Authenticate](../authenticate)
* Shared dependency deduplication → [Share Dependencies Across Widgets](../share-dependencies)
* Missing-file error text and recovery → [Troubleshooting](troubleshooting)
* Command flags → [Command reference](commands)
