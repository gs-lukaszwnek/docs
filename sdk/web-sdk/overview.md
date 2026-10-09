---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/overview.md
description: >-
  Overview of the Community Hub Web SDK (sdk.web) — the browser-side JavaScript
  API for searching content, managing users, and subscribing to topics from
  community pages.
---

# Web SDK

The **Community Hub Web SDK** is a JavaScript API exposed on community pages. It lets you search content, manage users, subscribe to topics and categories, and interact with the DOM from custom widgets or third-party scripts running in the community frontend. In widgets you reach it as `sdk.web`; in scripts you use the global `window.ChWebSdk`.

The SDK is **browser-only**: all methods that call the backend or DOM throw if run outside a browser environment.

## Availability

The SDK is bundled with the community frontend. There is no separate npm package.

* **In widgets**, read it from `sdk.web` on the object passed to your widget's `init(sdk)` function. `sdk.web` is `undefined` on host pages that do not load the community frontend, such as embedded widgets on external sites and Skilljar, so check it before use.
* **In [script extensions](/custom-widgets/v2/scripts-overview)** and other code outside a widget, which receive no `sdk` object, use the global `window.ChWebSdk`. This is not deprecated there.

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  console.log('Web SDK available')
}
```

::: tip
**No `onReady` in widget code.** `sdk.web.onReady` waits for the page document to load, not for the SDK. Inside `init(sdk)`, after `await sdk.whenReady()`, the document has already loaded, so call `sdk.web` methods directly. Use `window.ChWebSdk.onReady` only in [script extensions](/custom-widgets/v2/scripts-overview).
:::

::: warning Deprecated: `ChWebSdk` in widget code
Using `window.ChWebSdk` (or bare `ChWebSdk`) inside a widget still works, but is no longer recommended. Use `sdk.web` from your widget's `init(sdk)` function instead. `sdk.web` is the same object, with the same namespaces and methods.
:::

| Before (widget code) | After |
|----------------------|-------|
| `ChWebSdk.onReady(() => { ... })` | Not needed — put the code directly in `init(sdk)` after `await sdk.whenReady()` |
| `ChWebSdk.Content.search(...)` | `sdk.web.Content.search(...)` |
| `ChWebSdk.Context.User()` | `sdk.web.Context.User()` |
| `ChWebSdk.User.list(...)` | `sdk.web.User.list(...)` |
| `ChWebSdk.Subscription.subscribeToTopic(...)` | `sdk.web.Subscription.subscribeToTopic(...)` |

## Quick start

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  const results = await sdk.web.Content.search('feature request', { limit: 10 })
  console.log(results)
}
```

See [Examples](examples) for more runnable snippets covering search, subscriptions, users, and DOM interactions.

## Summary

* [DOM and lifecycle](methods-constructors#dom-and-lifecycle) — wait for the page or an element, bind events, load scripts and styles
* [Content Methods](methods-constructors#content-methods) — search, like, vote, and manage articles, questions, ideas, and more
* [Context Methods](methods-constructors#context-methods) — fetch the current viewer's user object (replaces `inSidedData.user`)
* [User Methods](methods-constructors#user-methods) — look up, search, list, and filter community members
* [Subscription Methods](methods-constructors#subscription-methods) — subscribe to topics and categories

## Next Steps

* [User Context](user-context) — complete reference for `Context.User()`, all User methods, the User object schema, field availability, and examples
* [Reference](methods-constructors) — DOM utilities, Content, User, Subscription, search options, types, and error handling
* [Examples](examples) — search, subscriptions, users, DOM, and script/style loading
