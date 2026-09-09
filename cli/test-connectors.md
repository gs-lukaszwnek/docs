---
url: https://developer-portal.gainsight.com/docs/cli/test-connectors.md
description: >-
  Run a Connector against your tenant from the CLI to verify request shape,
  auth, and payload rendering before you use it from a widget.
---

# Test Connectors

Use this guide when you want to exercise a Connector end-to-end from your terminal — inspect the outgoing request, confirm the tenant returns the response shape you expect, and iterate on request parameters without wiring the Connector into a widget first.

## Prerequisites

* An active session — see [Authenticate](authenticate)
* Either a local Connector defined under `widgets/<widget>/connectors.json`, or a Connector already deployed to your tenant

## Run a Connector

```sh
gsds connector test revenue-feed
```

Every run reports where the Connector was resolved from:

* `source: local` — the Connector was found under `widgets/*/connectors.json` in your project
* `source: remote` — the Connector was fetched from the tenant your session is paired with

Resolution is **local-first**. If a name matches a local Connector, `gsds` uses that even when the tenant has a Connector by the same name — this lets you iterate locally without publishing.

## Provide a request body

Pass a JSON body from a file. `--payload` accepts **file references only**, prefixed with `@`, and the file must contain valid JSON.

```sh
gsds connector test revenue-feed --payload @body.json
```

Inline JSON is rejected — put the payload in a file and reference it.

## Set query and path parameters

```sh
gsds connector test revenue-feed --query limit=5 --query cursor=abc
gsds connector test users --path-param user_id=42
```

Repeat `--query` and `--path-param` for each parameter.

## See the rendered upstream request

Add `--verbose` to print the request `gsds` sends upstream, after **Payload Templates** have been rendered and headers applied. Use this when a Connector "looks right" locally but returns an unexpected response.

```sh
gsds connector test revenue-feed --verbose
```

## Where runs are captured

Every run is written under `.gsds/` in your project root, with sensitive request headers redacted. `gsds init` gitignores `.gsds/` for you.

Use the capture directory to compare runs across changes, share reproduction cases without leaking credentials, and diff request shape after editing a Connector.

## Limitations

* **Composite Connectors cannot be tested.** `gsds connector test` runs a single Connector; composite orchestration happens at request time inside a widget. To validate a composite, test each leaf Connector individually.

## Next steps

* Drive `gsds connector test --json` from CI or an AI agent in [Automate with CI and AI](automate-with-ci-and-ai)
* Look up every flag in the [Command reference](reference/commands)
* Review the Connector data model in [Connectors](/connectors/)
