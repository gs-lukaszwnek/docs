---
url: https://developer-portal.gainsight.com/docs/cli/getting-started.md
description: >-
  Install the Developer Studio CLI, pair a session with your community, scaffold
  your first widget, and preview it live.
---

# Get Started with the CLI

In this tutorial, you will install the **Developer Studio CLI**, pair it with your community, scaffold a React widget, and preview it inside your community's No-Code Builder. By the end, you will have a working local development loop and know how to iterate on a widget without publishing anything.

## Before you begin

You need:

* Node.js 18 or later
* A community account with developer access
* Access to **CLI Access** under **Integrations → Developer Studio** in your community, so you can get a pairing code

## 1. Install the CLI

Install `gsds` globally with npm:

```sh
npm install -g @gainsight-hub/developer-studio-cli
gsds --version
```

You should see a version like `1.8.2` or newer.

## 2. Set up your project directory

`gsds init` handles two situations. Use whichever matches you.

**Starting from scratch**, with an empty or nonexistent directory:

```sh
gsds init acme-widgets
cd acme-widgets
```

**Reusing an existing repo** — for example one cloned from the [Widgets Repository Template](https://github.com/gainsight-hub/widgets-repository-template), or any project that already has `extensions_registry.json`, `connectors_registry.json`, or a `gsds.json`:

```sh
cd acme-widgets
gsds init acme-widgets
```

`gsds init` recognizes that shape automatically — no extra flag needed — and backfills only whatever's missing (`gsds.json`, `AGENTS.md`, `README.md`, `.gitignore`, and the registries if absent). It never overwrites a file it finds, so nothing you already have is at risk.

Only reach for `--force` when the directory is non-empty **and** doesn't already look like a widget project — for instance a brand-new GitHub repo created with just a `README.md` or `LICENSE`. `--force` means "this directory is safe to scaffold into, trust me"; it does not loosen the never-overwrite guarantee, and it isn't the default answer for "I already have a repo" — most existing widget repos are recognized without it. See [`gsds init`](reference/commands#gsds-init) in the command reference for the exact list of what's recognized.

## 3. Pair a session with your community

In your community admin, open **Integrations → Developer Studio → CLI Access** and copy the pairing code shown there. Then redeem it in your terminal:

```sh
gsds login PAIRING-CODE-FROM-COMMUNITY
```

Pairing codes are single-use and expire **1 minute** after they are issued, so open **CLI Access** with your terminal already open. The resulting session token is stored in your OS keychain, is tenant-scoped, and lasts **8 hours**.

Use the same account you sign in to your community with. Preview sessions are matched per user, so a session paired under one account is not visible to another in the No-Code Builder.

You can confirm the session at any time:

```sh
gsds login --status
```

## 4. Scaffold a widget

From your project root, scaffold a React widget:

```sh
gsds create --name revenue-overview --framework react --category Analytics
```

`gsds create` writes a new folder under `widgets/revenue-overview/` with a `package.json` that includes a `dev` script (required for previewing) and starter source files, and appends the widget's entry to `extensions_registry.json`.

There is no per-widget `widget.json`. `extensions_registry.json` is where a widget is declared — a folder the registry doesn't mention is invisible to your community, and `gsds build` warns when it finds one.

## 5. Preview it in your community

Start the local dev server:

```sh
gsds preview
```

`gsds preview` requires an active session (you just paired one). It boots each widget's `dev` script and registers a preview session with your community.

### Allow the browser to reach your local server

Do this before you open the picker. `gsds preview` serves from `http://localhost:5173` by default, and each widget with its own `dev` script serves from a further port. Your community page — served over HTTPS — loads from those addresses directly, and Chrome 142 and later ask permission the first time, with a prompt about looking for and connecting to devices on your local network. Other Chromium-based browsers behave the same way.

Choose **Allow**. Until it is allowed, the browser cannot reach your local server and your local widgets do not load. If you blocked it by mistake, reopen the choice from the site settings icon at the left of the address bar and reload the page.

### Open the picker

Open your community, browse to a page, and open the widget picker in the No-Code Builder. Your local widgets appear alongside published ones while `gsds preview` is running. Edit files under `widgets/revenue-overview/` and the picker reflects your changes.

If a widget does not appear, work through [Widget missing from the picker](reference/troubleshooting#widget-missing-from-the-picker).

Stop the preview with `Ctrl-C`.

## What you learned

* How to install `gsds` and confirm it works
* How to pair a session with a pairing code, under the account you browse the community with
* How to scaffold a project and a widget
* How to preview local widgets against your community

## Next steps

* Learn the full session model — pairing, expiry, and logout — in [Authenticate](authenticate)
* Add a Connector and run it against your tenant with [Test Connectors](test-connectors)
* See every command and flag in the [Command reference](reference/commands)
* Match an error message or a silent failure to its cause in [Troubleshooting](reference/troubleshooting)
