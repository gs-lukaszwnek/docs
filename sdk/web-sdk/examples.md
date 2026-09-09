---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/examples.md
description: >-
  Runnable code examples for the Web SDK — search, subscriptions, users, DOM
  observation, and script loading.
---

# Examples

Use these ready-made code snippets to add search, user personalization, and dynamic content to your community pages. Each example is self-contained -- copy it into a community page and it works as-is.

::: info Prerequisites
These examples run on a community page where the community frontend bundle has loaded, making `ChWebSdk` globally available. See [Web SDK](overview) for availability details.
:::

## Search ideas and show titles

```javascript
ChWebSdk.onReady(async () => {
  const ideaInstance = ChWebSdk.Content.Idea()
  const results = await ideaInstance.search('dashboard', { limit: 5 })
  results.forEach((r) => console.log(r.title, r.url))
})
```

## Search with filters and pagination

```javascript
ChWebSdk.onReady(async () => {
  const results = await ChWebSdk.Content.search('bug', {
    contentType: ['idea', 'question'],
    categoryName: 'Ideas',
    limit: 20,
    page: 1,
    fetchMetadata: true
  })
  console.log(results)
})
```

## Like an idea (logged-in user)

```javascript
ChWebSdk.onReady(async () => {
  const idea = ChWebSdk.Content.Idea(98765)
  await idea.like()
})
```

## Subscribe to a topic and check status

```javascript
ChWebSdk.onReady(async () => {
  await ChWebSdk.Subscription.subscribeTopic('topic-abc')
  const isSubscribed = await ChWebSdk.Subscription.getTopicSubscriptionStatus('topic-abc')
  console.log(isSubscribed) // true
})
```

## Find users and use in UI

```javascript
ChWebSdk.onReady(async () => {
  const ids = ['1', '2', '3']
  const usersMap = await ChWebSdk.User.getUsersById(ids)
  ids.forEach((id) => {
    const u = usersMap[id]
    if (u) console.log(u.name, u.avatar)
  })
})
```

## Wait for an element and bind click

```javascript
ChWebSdk.observeElement('.custom-widget', (el) => {
  el.innerHTML = '<button class="my-btn">Click</button>'
  ChWebSdk.onEvent('.my-btn', 'click', () => {
    console.log('Clicked')
  })
})
```

## Load a script and style before using a widget

```javascript
ChWebSdk.loadStyle('https://example.com/widget.css')
ChWebSdk.loadScript('https://example.com/widget.js', () => {
  // Widget script is loaded
})
```

## Next Steps

* [Web SDK Methods and Constructors](methods-constructors) — DOM utilities, Content, User, Subscription methods, and error handling
* [Web SDK](overview) — how the SDK is loaded and when to use each API
