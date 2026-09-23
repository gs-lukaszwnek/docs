---
url: https://developer-portal.gainsight.com/docs/cli/automate-with-ci-and-ai.md
description: >-
  Drive the Developer Studio CLI from CI pipelines and AI agents using --json
  output, non-interactive flags, and exit codes.
---

# Automate with CI and AI

Use this guide when you want to run `gsds` unattended — in a CI pipeline, from an AI agent, or from a script — and need every input as a flag and every output as machine-readable JSON.

## The automation contract

Two guarantees make `gsds` scriptable:

* **Every interactive path has a flag equivalent.** In a non-TTY environment, commands that would prompt fail fast instead, and the error names which flag is missing.
* **Every human output has `--json`.** Add `--json` to any command that prints information and you get a stable, documented structure instead of a formatted table.

## Exit codes

Commands exit `0` on success and `1` on any failure. Treat non-zero as "failed" — do not branch on the specific value. Finer-grained exit codes may land in a minor version, so scripts that switch on the exact number will break.

## Non-interactive scaffolding

In a TTY, `gsds create` prompts for name, framework, and category. In CI or under an AI agent, all three flags are required:

```sh
gsds create --name revenue-overview --framework react --category Analytics
```

If any of the three is missing, the command fails and names the missing flag — no partial widget is written.

Supported frameworks: `react`, `vue`, `angular`, `vanilla`, `html`.

## Machine-readable listings

`gsds list --json` returns an object keyed by Extension type:

```sh
gsds list --json
```

The top level is `{ widgets, connectors, scripts, stylesheets }`, not a bare array. Scripts that expect an array will fail — always index into the keyed object.

Example: preview every previewable widget:

```sh
gsds list --json \
  | jq -r '.widgets[] | select(.previewable) | .type' \
  | xargs gsds preview --widget
```

## Machine-readable Connector runs

```sh
gsds connector test revenue-feed --query limit=5 --json
```

Pair `--json` with `--query`, `--path-param`, and `--payload @body.json` for a fully non-interactive Connector run. Use this shape for smoke tests in CI.

## Validate the registries in CI

`gsds build --validate` compares `extensions_registry.json` and `connectors_registry.json` against the source files and exits `1` on drift. Wire it into your pipeline as a gate:

```sh
gsds build --validate
```

`--validate` does not rebuild `dist/`, so run plain `gsds build` locally before committing — otherwise CI catches drift you introduced yourself.

## Sessions in CI

`gsds preview` and `gsds connector test` require a paired session. There is no headless login flow — pairing codes come from **Integrations → Developer Studio → CLI Access** in the community and expire in 1 minute. Options for CI:

* Keep those commands out of the pipeline (recommended). Run them locally during development, and gate CI on `gsds build --validate` instead.
* Run pairing on a bootstrap machine and rely on the 8-hour session, understanding it will expire.

If a bootstrap machine holds sessions for more than one tenant, pass `--profile <name>` on `gsds connector test` or `gsds preview` to target a specific one without switching which profile is current — see [Authenticate](authenticate#use-multiple-tenants).

## Force the file-based credential fallback

Set `GSDS_DISABLE_KEYCHAIN=1` in environments where no OS keychain is available. The CLI writes to `~/.config/gsds/credentials.json` at file mode `0600` and warns once on stderr. See [Project Files](reference/project-files#in-your-user-account) for the full session-storage rules.

## Control the automatic update

`gsds` checks npm for a newer version roughly every 8 hours and installs it without asking, applying to the command you just ran. Set `GSDS_DISABLE_AUTO_UPDATE=1` to disable this entirely — no registry call, no install, no cache write:

```sh
export GSDS_DISABLE_AUTO_UPDATE=1
```

Set it anywhere an unattended global npm install would be unwelcome or fail:

* **CI pipelines**, where the CLI version should be pinned by your lockfile or install step, not changed underneath a build.
* **Sandboxed or offline runners** with no registry access, or a read-only `HOME`.
* **Agent-driven invocations**, where a global install racing other work is a hazard.

A failed check on its own — network error, missing `npm`, unwritable config directory — never fails your command; it is swallowed with at most a one-line warning, and your command runs on the current version. `GSDS_DISABLE_AUTO_UPDATE` is for skipping the check outright, not for handling that failure.

## Next steps

* Every flag and its default in the [Command reference](reference/commands)
* The full list of committed and generated files in [Project files](reference/project-files)
* Session model and profiles in [Authenticate](authenticate)
