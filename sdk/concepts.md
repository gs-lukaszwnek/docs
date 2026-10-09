---
url: https://developer-portal.gainsight.com/docs/sdk/concepts.md
description: >-
  How the single sdk object passed to init(sdk) combines the widget runtime with
  the sdk.connectors and sdk.web namespaces, and why each is a separate layer
---

# SDK Concepts

Your widget's `init(sdk)` function receives one `sdk` object. It is the single entry point to everything the platform offers widget code. This page explains what is on that object, why it is split into separate layers, and which page to go to for each.

## One `sdk` object, three layers

| | What it is | How you access it | What it's for |
|---|---|---|---|
| **[Widget Runtime Reference](/sdk/runtime-reference)** | The lifecycle contract — see [Widget Runtime](/custom-widgets/v2/core-concepts) for how it works | Directly on `sdk`, for example `sdk.getContainer()` and `sdk.getProps()` | Gives your widget its shadow root, current props, design tokens, and events |
| **[Connectors SDK](/sdk/connectors-sdk/overview)** | A backend proxy client, shared by every widget on the page | `sdk.connectors` | Call [Connectors](/connectors/) to reach external APIs without exposing credentials in the browser |
| **[Web SDK](/sdk/web-sdk/overview)** | The community-platform client | `sdk.web` — `undefined` on host pages that publish no Web SDK | Search content, manage users and subscriptions, and interact with the DOM on community pages |

Nothing on `sdk` needs to be constructed or loaded: it already exists by the time your `init` function runs, the same way React hands a component its `props` or Express hands a handler `req`/`res`.

`sdk.web` is `undefined` where the host page publishes no Web SDK — embedded widgets on non-community host pages, and Skilljar. Check it before use:

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  // ...
}
```

## Why they're kept separate

Each layer talks to something different, so collapsing them would mix unrelated concerns:

* **`sdk.connectors`** is a backend proxy client. Calling an external API directly from the browser would expose your credentials, so this client exists purely to route requests through the platform instead, keeping credentials on the server. It is one client shared by every widget on the page, so configuration changes made through it apply to all of them.
* **`sdk.web`** is a community-platform client. It has nothing to do with external APIs — it searches and manages content that already lives in the community itself.
* **The runtime** (the rest of `sdk`) isn't a client for anything external. It's the contract between the platform and your widget's lifecycle: where to render, what configuration you were given, and what to do when that configuration changes.

A widget can use all three at once — read its props from the runtime, call a connector through `sdk.connectors`, and search community content through `sdk.web` — without any of the three needing to know the others exist.

## Older code: the window globals

Earlier widget code reached these through globals: `new window.WidgetServiceSDK()` for connectors and `window.ChWebSdk` for the Web SDK. Both are deprecated. They still work, but use `sdk.connectors` and `sdk.web` from `init(sdk)` instead.

The SDKs are available only through the `sdk` object your widget's `init(sdk)` receives. Script extensions and other page scripts have no SDK access — if you need community data or a connector, build a [widget](/custom-widgets/v2/build-first-widget).

## What's Next

* [Your First Widget](/custom-widgets/v2/build-first-widget) — build a widget hands-on and see the `init(sdk)` parameter in action
* [Connectors SDK](/sdk/connectors-sdk/overview) — call connectors with `sdk.connectors`
* [Web SDK](/sdk/web-sdk/overview) — search, users, and subscriptions with `sdk.web`
* [Widget Runtime Reference](/sdk/runtime-reference) — full API for the `init(sdk)` parameter
