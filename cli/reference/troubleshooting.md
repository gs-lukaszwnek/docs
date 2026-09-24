---
url: https://developer-portal.gainsight.com/docs/cli/reference/troubleshooting.md
description: >-
  Every documented Developer Studio CLI error message, what it means, and how to
  recover.
---

# Troubleshooting

Reference of the error messages `gsds` prints and the state each one describes.

## `Directory "<path>" is not empty`

`gsds init` didn't recognize the target directory as an existing widget project — none of `extensions_registry.json`, `connectors_registry.json`, `gsds.json`, or the legacy `widget_registry.json` are present — and it isn't empty.

**Recover:** If the directory already has project files that should have matched one of the names above (perhaps under a slightly different name), fix that first. Otherwise, if you're sure the directory is safe to scaffold into, re-run with `--force`. See [`gsds init`](commands#gsds-init) for the full list of what's recognized without it.

## `no gsds.json found`

`gsds` is running outside a project. Every command except `gsds init` requires a `gsds.json` marker in the current directory or an ancestor.

**Recover:** `cd` into your project root, or scaffold a new project with `gsds init <name>`.

## `session is invalid or expired`

The stored session token failed against your tenant. Session tokens are valid for 8 hours and cannot be refreshed. Affects `gsds preview` and `gsds connector test`.

**Recover:** Get a fresh pairing code from **Integrations → Developer Studio → CLI Access** in your community and redeem it with `gsds login <pairing_code>`. Confirm with `gsds login --status`.

## `Stored login has expired. Run gsds login <pairing_code> to re-authenticate.`

`gsds preview` found a stored session for the profile, but it has timed out. This is distinct from `Run gsds login <pairing_code> first.`, which means nothing is stored for that profile at all.

**Recover:** Redeem a fresh pairing code under the same profile. `gsds profile` marks an expired profile `(expired)`, and one whose stored credential cannot be read `(unreadable)`, so you can see which one to log back into.

## A profile is marked `(unreadable)`

`gsds profile` lists the profile, but its stored credential cannot be read back — the entry exists and its contents are corrupted, hand-edited, or restored from a backup that no longer parses. It is not a usable session, and it is reported separately from `(expired)` because nothing about it can be read, including whether it had timed out.

**Recover:** Same as an expired session — redeem a fresh pairing code under that profile with `gsds login <pairing_code> --profile <name>`.

## Pairing code expired

Pairing codes have a 1-minute time-to-live.

**Recover:** Get a fresh pairing code from **CLI Access** and redeem it immediately, with your terminal already open.

## A registry file is missing

`extensions_registry.json` or `connectors_registry.json` is gone from a project that previously had it. `gsds build` refuses rather than writing an empty registry: the file is the source of truth, not generated output, so rebuilding cannot recover the titles, categories, configuration, and types it held.

**Recover:** Restore the file from version control. To deliberately start over, create it yourself with an empty `{"widgets": []}` (or `{"connectors": []}`).

## `Missing required field: category`

A widget entry in `extensions_registry.json` is missing a required field. `gsds build` refuses to write the registry until every entry declares a non-empty `title` and `category`.

**Recover:** Add a `category` string to the entry named in the error.

## `Script "x" points at missing file`

An `extensions_registry.json` entry references a file that no longer exists in the project. `gsds build` refuses to write the registry while it is inconsistent.

**Recover:** Either restore the missing file, or remove the stale entry with `gsds script rm <name>` or `gsds style rm <name>`. `gsds create` warns on the same drift without failing, so you can scaffold new widgets while cleaning up.

## `widget.json ignored, no longer used, safe to delete`

A leftover `widget.json` or `connectors.json` inside a widget directory. Both registries are the sole source of truth now, and `gsds build` never reads either file.

**Recover:** Delete the file. This message only appears when the widget already has an entry in `extensions_registry.json`, so nothing is lost with it.

## `widget.json found but not in extensions_registry.json`

The same leftover file, for a widget with no registry entry. Here the file is the widget's only remaining metadata, so deleting it loses the widget.

**Recover:** Copy the widget's `title`, `category`, `type`, and `source` or `content` into an `extensions_registry.json` entry, then delete the file.

## `widgets/<name>/ has no entry in extensions_registry.json`

A widget directory the registry never mentions. The platform only serves what the registry declares, so this widget will never appear in your community.

**Recover:** Add an entry for it to `extensions_registry.json`, or delete the folder if it is not a widget.

## `version mismatch across widgets`

Two widgets declare the same dependency in `package.json` but resolve it to different installed versions. `gsds build` externalizes a shared dependency into one import map entry, so it cannot serve two versions at once.

**Recover:** The error names each widget and version. Align the versions, or remove the dependency from one widget, then re-run `gsds build`. See [Share Dependencies Across Widgets](../share-dependencies).

## `excluded from shared-dependency deduplication`

A widget's `vite.config.ts` (or `angular.json`) couldn't be auto-migrated to the externalized-dependency shape. This only happens once the widget actually shares a dependency with another widget, and its build file has a shape `gsds build` can't confidently patch — a custom `rollupOptions`, or a build block laid out differently than the standard template.

**Recover:** Copy the `externalPackages` block from a freshly scaffolded widget's `vite.config.ts` into the named widget's file. See [Share Dependencies Across Widgets](../share-dependencies).

## `--payload` rejected

`--payload` accepts a file reference only. Inline JSON, and files that do not contain valid JSON, are rejected.

**Recover:** Put the payload in a file and pass it as `--payload @body.json`. Confirm the file parses as JSON.

## Port in use

`gsds preview` failed to bind its port. The default is `5173`.

**Recover:** `gsds preview --port <port>` on a free port.

## The browser blocks the local preview

`gsds preview` serves from `http://localhost:5173` by default, and each JavaScript widget's own `dev` script serves from a further port. Your community page — served over HTTPS — loads from those addresses directly. Chrome 142 and later treat that as a local network request and ask permission the first time, with a prompt about looking for and connecting to devices on your local network. Other Chromium-based browsers behave the same way. Granting it once covers every port your preview uses.

**Recover:** Choose **Allow** when the prompt appears. If you already chose to block it, reopen the choice from the site settings icon at the left of the address bar and allow local network access for your community's address, then reload the page. Until it is allowed, the browser cannot reach your local server and your local widgets do not load.

## Widget missing from the picker

`gsds preview` is running but a widget does not appear in the No-Code Builder.

**Recover:** Check three things, in order.

1. **The widget has a registry entry.** `gsds build` never derives entries from a folder scan, so a `widgets/<name>/` folder the registry doesn't declare is invisible.
2. **The widget declares a `dev` script** in its `package.json`, or its registry entry declares a `source` so the preview server can serve it directly. A widget with neither is skipped, and `gsds preview` says so.
3. **You are signed in to the community as the same account that ran `gsds login`.** The No-Code Builder looks for a preview session belonging to the account you are signed in as, so a session paired under a different account is not visible to you. Confirm the paired account with `gsds login --status`.

## A preview asset 404s or loads the wrong file

A widget served as static HTML references an asset that does not load under `gsds preview`.

**Recover:** Resolve references the way publishing does. A reference that starts with a slash — a root-absolute reference, such as `/app.js` — resolves against the widget root, `widgets/<name>/app.js`, not against the directory its HTML sits in. A reference naming no file in the widget is left exactly as written and warned about once, so check that warning for a typo before assuming the server is at fault.

## `Per-widget build failed`

One or more widgets could not be prepared, and `gsds build` names each one. The most common cause is a dependency install that failed: `gsds build` installs each widget's dependencies automatically, leaves a widget whose install failed out of the rebuild pass, and then fails the whole run rather than shipping a stale `dist/` for it. The widget's registry entry is untouched — the registry is hand-authored, and no build path removes an entry.

**Recover:** Run `npm install` (or your package manager's equivalent) inside the named widget's directory to see the underlying error. Fix it, then re-run `gsds build`.

## Related

* Session model → [Authenticate](../authenticate)
* Where `.gsds/` and the keychain fit in → [Project files](project-files)
* Flags for each command → [Command reference](commands)
* Shared dependency deduplication → [Share Dependencies Across Widgets](../share-dependencies)
