---
url: https://developer-portal.gainsight.com/docs/cli.md
description: >-
  Scaffold, preview, and validate Custom Widgets, Connectors, and sitewide
  assets from your terminal with the Developer Studio CLI.
---

# CLI

The **Developer Studio CLI** (`gsds`) is the command-line companion to the Developer Portal. Use it to scaffold projects, preview widgets against your community, test Connectors, and manage sitewide scripts and stylesheets, all without leaving your terminal.

Distributed as `@gainsight-hub/developer-studio-cli` on npm. Requires Node.js 18 or later.

## Where to start

| If you want to... | Go to |
|---|---|
| Install `gsds` and preview your first widget end-to-end | [Get started](getting-started) |
| Pair a session with your community | [Authenticate](authenticate) |
| Run a Connector against your tenant from the CLI | [Test Connectors](test-connectors) |
| Register sitewide scripts and stylesheets | [Manage scripts and stylesheets](manage-scripts-and-styles) |
| Drive `gsds` from CI or an AI agent | [Automate with CI and AI](automate-with-ci-and-ai) |
| Look up a command, flag, or exit code | [Command reference](reference/commands) |
| See which files `gsds` reads and writes | [Project files](reference/project-files) |
| Recognize a specific error message | [Troubleshooting](reference/troubleshooting) |

## What `gsds` is not

`gsds` does not publish widgets. Publishing is triggered by pushes to your **watched branch** — the CLI is a local development tool. See [Extensions](/custom-widgets/v2/) for the publish flow.
