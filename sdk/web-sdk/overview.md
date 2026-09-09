---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/overview.md
description: >-
  Overview of the Community Hub Web SDK (ChWebSdk) — the browser-side JavaScript
  API for searching content, managing users, and subscribing to topics from
  community pages.
---

# Web SDK

The **Community Hub Web SDK** (`ChWebSdk`) is a JavaScript API exposed on community pages. It lets you search content, manage users, subscribe to topics and categories, and interact with the DOM from custom widgets or third-party scripts running in the community frontend.

The SDK is **browser-only**: all methods that call the backend or DOM throw if run outside a browser environment.

## Availability

The SDK is bundled with the community frontend and attached to `window` (or `globalThis`) when the destination app loads. There is no separate npm package; you use the global `ChWebSdk` object on pages that include the community bundle.

```javascript
// After the community app has loaded
ChWebSdk.onReady(() => {
  console.log('SDK ready')
})
```

## Quick start

```javascript
ChWebSdk.onReady(async () => {
  const results = await ChWebSdk.Content.search('feature request', { limit: 10 })
  console.log(results)
})
```

See [Examples](examples) for more runnable snippets covering search, subscriptions, users, and DOM interactions.

## Summary

* [DOM and lifecycle](methods-constructors#dom-and-lifecycle) — wait for the page or an element, bind events, load scripts and styles
* [Content Methods](methods-constructors#content-methods) — search, like, vote, and manage articles, questions, ideas, and more
* [User Methods](methods-constructors#user-methods) — search users and fetch by ID
* [Subscription Methods](methods-constructors#subscription-methods) — subscribe to topics and categories

## Next Steps

* [Web SDK Methods and Constructors](methods-constructors) — DOM utilities, Content, User, Subscription, search options, types, and error handling
* [Examples](examples) — search, subscriptions, users, DOM, and script/style loading
