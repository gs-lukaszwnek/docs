---
url: https://developer-portal.gainsight.com/docs/cli/authenticate.md
description: >-
  Pair the Developer Studio CLI with your community using a pairing code, check
  the current session, and log out.
---

# Authenticate

Use this guide when you need to pair `gsds` with your community for the first time, refresh an expired session, confirm which tenant you are paired with, or sign out of a shared machine.

## Pair a session

Get a pairing code from your community:

1. In the community admin, open **Integrations** from the sidebar.
2. Under the **Developer Studio** heading, choose **CLI Access**.
3. Copy the pairing code from that page.

Then, in your terminal, redeem it:

```sh
gsds login PAIRING-CODE-FROM-COMMUNITY
```

Pairing codes have a **1-minute** time-to-live, so open **CLI Access** with your terminal already open. If the code expires before you paste it, refresh **CLI Access** and try again.

On success, `gsds` stores a tenant-scoped session token that is valid for **8 hours**.

## Check the current session

```sh
gsds login --status
```

Reports which tenant the current session is bound to and whether it is still valid. Use this first whenever you see a `401` from `gsds preview` or `gsds connector test`.

## Refresh an expired session

There is no refresh flow — when the 8-hour token expires, get a new pairing code from **Integrations → Developer Studio → CLI Access** and redeem it:

```sh
gsds login NEW-PAIRING-CODE
```

## Log out

Clear the stored session:

```sh
gsds logout
```

Run this before handing a shared workstation to another developer, or when switching between tenants.

## Where the session is stored

`gsds` writes the session token to your OS keychain — macOS Keychain, Windows Credential Store, or Linux Secret Service / keyutils — under service `gainsight-developer-studio-cli`, account `default`.

If the keychain is unavailable (no supported provider, headless CI, or a keychain error), `gsds` falls back to `~/.config/gsds/credentials.json` at file mode `0600` and prints a one-time warning on stderr. You can force the file fallback by setting `GSDS_DISABLE_KEYCHAIN=1`.

## Commands that require a session

| Command | Why it needs a session |
|---|---|
| `gsds preview` | Registers a preview session with your community so local widgets appear in the picker |
| `gsds connector test` | Runs against the paired tenant |

Every other command works without a session.

## Next steps

* Test a Connector against the paired tenant in [Test Connectors](test-connectors)
* Run `gsds` unattended in CI or from an AI agent with [Automate with CI and AI](automate-with-ci-and-ai)
* Look up flags for the login commands in the [Command reference](reference/commands)
