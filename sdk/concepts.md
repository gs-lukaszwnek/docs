---
url: https://developer-portal.gainsight.com/docs/sdk/concepts.md
description: >-
  Why these docs use the word SDK for three different things — two constructed
  libraries (Widget SDK, Web SDK) and one runtime context object — and how they
  relate
---

# SDK Concepts

The word "SDK" shows up in three places in these docs, but only two of them are actually SDKs. This page explains what each one is, why they're kept separate, and which page to go to for each.

## Two SDKs and one runtime context object

| | What it is | How you access it | What it's for |
|---|---|---|---|
| **[Widget SDK](/sdk/widget-sdk/overview)** | A JavaScript class you construct | `new window.WidgetServiceSDK()`, called from inside your widget code | Call [Connectors](/connectors/) to reach external APIs without exposing credentials in the browser |
| **[Web SDK](/sdk/web-sdk/overview)** | A global object already on the page | `ChWebSdk` — bundled with the community frontend, no construction needed | Search content, manage users and subscriptions, and interact with the DOM on community pages |
| **[Widget Runtime](/custom-widgets/v2/core-concepts)** (the `sdk` parameter) | A context object, not a library | Passed automatically as the argument to your widget's `init(sdk)` function — see the [Widget Runtime Reference](/custom-widgets/v2/sdk-api-reference) | Gives your widget its shadow root, current props, and design tokens |

The first two are libraries you call, the same way you'd call any client SDK: you construct or reference an instance, then invoke methods on it. The third is not — it's an interface the platform hands you, the same way React hands a component its `props` or Express hands a handler `req`/`res`. Nothing about it is optional or constructed; it already exists by the time your `init` function runs.

## Why they're kept separate

Each one talks to a different layer, so collapsing them would mix unrelated concerns:

* **Widget SDK** is a backend proxy client. Calling an external API directly from the browser would expose your credentials, so this SDK exists purely to route requests through the platform instead.
* **Web SDK** is a community-platform client. It has nothing to do with external APIs — it searches and manages content that already lives in the community itself.
* **Widget Runtime** isn't a client for anything external. It's the contract between the platform and your widget's lifecycle: where to render, what configuration you were given, and what to do when that configuration changes.

A widget can use all three at once — read its props from the runtime object, call a connector through the Widget SDK, and search community content through the Web SDK — without any of the three needing to know the others exist.

## Common confusion

The runtime object and the Widget SDK are easy to mix up because widget code often names both of them `sdk`. The runtime object has no `.connectors` property, so calling `sdk.connectors.execute(...)` inside `init(sdk)` throws `Cannot read properties of undefined (reading 'execute')`. See [Calling from Widget Code](/connectors/calling-from-widgets) for how to construct a separate, distinctly-named Widget SDK instance instead.

## What's Next

* [Your First Widget](/custom-widgets/v2/build-first-widget) — build a widget hands-on and see the `init(sdk)` parameter in action
* [Widget SDK](/sdk/widget-sdk/overview) — call connectors from widget code
* [Web SDK](/sdk/web-sdk/overview) — search, users, and subscriptions on community pages
* [Widget Runtime Reference](/custom-widgets/v2/sdk-api-reference) — full API for the `init(sdk)` parameter
