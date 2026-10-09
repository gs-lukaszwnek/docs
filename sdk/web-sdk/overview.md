---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/overview.md
description: >-
  Overview of the Community Hub Web SDK (sdk.web) — the browser-side JavaScript
  API for searching content, managing users, and subscribing to topics from
  community pages.
---

# Web SDK

The **Community Hub Web SDK** is a JavaScript API exposed on community pages. It lets you search content, manage users, subscribe to topics and categories, and load scripts from custom widgets running in the community frontend. You reach it as `sdk.web` on the `sdk` object your widget's `init(sdk)` receives.

The SDK is **browser-only**.

## Availability

The SDK is bundled with the community frontend. There is no separate npm package.

Read it from `sdk.web` on the object passed to your widget's `init(sdk)` function. `sdk.web` is `undefined` on host pages that do not load the community frontend, such as embedded widgets on external sites and Skilljar, so check it before use. The SDKs are available only through the `sdk` object your widget's `init(sdk)` receives. Script extensions and other page scripts have no SDK access — if you need community data or a connector, build a [widget](/custom-widgets/v2/build-first-widget).

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  console.log('Web SDK available')
}
```

::: warning Deprecated: `window.ChWebSdk`
`window.ChWebSdk` still works, but is no longer supported for new code. Use `sdk.web` from your widget's `init(sdk)` function instead. `sdk.web` has the same namespaces and methods.
:::

| Before | After |
|----------------------|-------|
| `ChWebSdk.onReady(() => { ... })` | Not needed — put the code directly in `init(sdk)` after `await sdk.whenReady()` |
| `ChWebSdk.observeElement(...)` / `ChWebSdk.onEvent(...)` | Not available in widgets — query your widget's own markup with `sdk.$()` and attach listeners to those elements |
| `ChWebSdk.loadStyle(...)` | Not available — put styles in your widget's own CSS |
| `ChWebSdk.loadScript(...)` | `sdk.web.loadScript(...)` |
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

See [Examples](examples) for more runnable snippets covering search, subscriptions, users, and script loading.

## Summary

* [Loading scripts](methods-constructors#loading-scripts) — load an external script by URL
* [Content Methods](methods-constructors#content-methods) — search, like, vote, and manage articles, questions, ideas, and more
* [Context Methods](methods-constructors#context-methods) — fetch the current viewer's user object (replaces `inSidedData.user`)
* [User Methods](methods-constructors#user-methods) — look up, search, list, and filter community members
* [Subscription Methods](methods-constructors#subscription-methods) — subscribe to topics and categories

## Next Steps

* [User Context](user-context) — complete reference for `Context.User()`, all User methods, the User object schema, field availability, and examples
* [Reference](methods-constructors) — Loading scripts, Content, User, Subscription, search options, types, and error handling
* [Examples](examples) — search, subscriptions, users, and script loading
