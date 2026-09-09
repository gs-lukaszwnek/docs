---
url: https://developer-portal.gainsight.com/docs/cli/reference/troubleshooting.md
description: >-
  Every documented Developer Studio CLI error message, what it means, and how to
  recover.
---

# CLI Troubleshooting

Reference of the error messages `gsds` prints and the state each one describes.

## `no gsds.json found`

`gsds` is running outside a project. Every command except `gsds init` requires a `gsds.json` marker in the current directory or an ancestor.

**Recover:** `cd` into your project root, or scaffold a new project with `gsds init <name>`.

## `session is invalid or expired`

The stored session token failed against your tenant. Session tokens are valid for approximately 8 hours and cannot be refreshed. Affects `gsds preview` and `gsds connector test`.

**Recover:** Mint a fresh pairing code from **Integrations → Developer Studio → CLI Access** in your community and redeem it with `gsds login <pairing_code>`. Confirm with `gsds login --status`.

## Pairing code expired

Pairing codes have a 1-minute time-to-live.

**Recover:** Mint a fresh pairing code from **CLI Access** and redeem it immediately, with your terminal already open.

## `Missing required field: category`

A `widget.json` in your project is missing a required field. `gsds build` refuses to write the registry until every `widget.json` declares a non-empty `title` and `category`.

**Recover:** Add a `category` string to the `widget.json` named in the error.

## `Script "x" points at missing file`

A `extensions_registry.json` entry references a file that no longer exists in the project. `gsds build` refuses to regenerate the registry while it is inconsistent.

**Recover:** Either restore the missing file, or remove the stale entry with `gsds script rm <name>` or `gsds style rm <name>`. `gsds create` warns on the same drift without failing, so you can scaffold new widgets while cleaning up.

## `--payload` rejected

`--payload` accepts a file reference only. Inline JSON, and files that do not contain valid JSON, are rejected.

**Recover:** Put the payload in a file and pass it as `--payload @body.json`. Confirm the file parses as JSON.

## Port in use

`gsds preview` failed to bind its default port.

**Recover:** `gsds preview --port <port>` on a free port.

## Widget missing from the picker

`gsds preview` is running but a widget does not appear in the No-Code Builder.

**Recover:** Confirm the widget's `package.json` declares a `dev` script. `gsds preview` boots each widget's `dev` script — widgets without one are skipped.

## Related

* Session model → [Authenticate](../authenticate)
* Where `.gsds/` and the keychain fit in → [Project files](project-files)
* Flags for each command → [Command reference](commands)
