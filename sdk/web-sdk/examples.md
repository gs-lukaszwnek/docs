---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/examples.md
description: >-
  Runnable code examples for the Web SDK — search, subscriptions, users, DOM
  observation, and script loading.
---

# Examples

Use these ready-made code snippets to add search, user personalization, and dynamic content to your community pages. Each example is self-contained -- widget examples go in your widget's module script; script examples go in a script extension.

::: info Prerequisites
Widget examples run inside your widget's `init(sdk)` function on a community page where the community frontend has loaded, which makes `sdk.web` available. On other host pages `sdk.web` is `undefined`, so each example checks it first. Script examples have no `init(sdk)` and use `window.ChWebSdk`. See [Web SDK](overview) for availability details.
:::

## Search ideas and show titles

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const ideaInstance = sdk.web.Content.Idea()
    const results = await ideaInstance.search('dashboard', { limit: 5 })
    results.forEach((r) => console.log(r.title, r.url))
  })
}
```

## Search with filters and pagination

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const results = await sdk.web.Content.search('bug', {
      contentType: ['idea', 'question'],
      categoryName: 'Ideas',
      limit: 20,
      page: 1,
      fetchMetadata: true
    })
    console.log(results)
  })
}
```

## Like an idea (logged-in user)

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const idea = sdk.web.Content.Idea(98765)
    await idea.like()
  })
}
```

## Subscribe to a topic and check status

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    await sdk.web.Subscription.subscribeToTopic('topic-abc')
    const isSubscribed = await sdk.web.Subscription.getTopicSubscriptionStatus('topic-abc')
    console.log(isSubscribed) // true
  })
}
```

## Get the current user (viewer context)

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const me = await sdk.web.Context.User()

    if (me.userId === null) {
      // Guest (guest projection) — show login prompt or public-only content
      console.log('Not logged in')
      return
    }

    // Authenticated viewer
    console.log(me.username)     // 'janedoe'
    console.log(me.userId)       // 101
    console.log(me.rank?.name)   // 'Regular'
    console.log(me.badges)       // [{ id: 100, title: 'First post', url: 'https://assets.example.com/…_thumb.png' }]
  })
}
```

## Show the current user's badges

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  await sdk.whenReady()
  sdk.web.onReady(async () => {
    const me = await sdk.web.Context.User()
    if (me.userId === null || me.badges.length === 0) return

    const list = document.createElement('ul')
    me.badges.forEach((badge) => {
      const li = document.createElement('li')
      li.textContent = badge.title
      list.appendChild(li)
    })
    sdk.$('#badges')?.appendChild(list)
  })
}
```

## Personalize based on profile fields

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  await sdk.whenReady()
  sdk.web.onReady(async () => {
    const me = await sdk.web.Context.User()
    if (me.userId === null) return

    // Find a profile field by ID (field IDs are specific to your community)
    const seniority = me.profileFields.find((f) => f.id === 3)
    if (seniority?.value === 'senior') {
      sdk.$('#advanced-content')?.removeAttribute('hidden')
    }
  })
}
```

## Find users and use in UI

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const users = await sdk.web.User.getUsersById([1, 2, 3])
    users.forEach((u) => console.log(u.userId, u.username, u.avatar))
  })
}
```

## List moderators

```javascript
export async function init(sdk) {
  if (!sdk.web) return // no Web SDK on this host page (e.g. embedded widgets, Skilljar)
  sdk.web.onReady(async () => {
    const { totalItems, items } = await sdk.web.User.list({
      role: [7], // role IDs are specific to your community
      sort: 'lastVisit',
      order: 'desc',
      size: 10
    })
    console.log(`${items.length} of ${totalItems} moderators`)
    items.forEach((u) => console.log(u.username, u.rank?.name))
  })
}
```

## Wait for an element and bind click (script example)

Scripts receive no `init(sdk)` object, so this example uses `window.ChWebSdk`.

```javascript
window.ChWebSdk.observeElement('.custom-widget', (el) => {
  el.innerHTML = '<button class="my-btn">Click</button>'
  window.ChWebSdk.onEvent('.my-btn', 'click', () => {
    console.log('Clicked')
  })
})
```

## Load a script and style before using a widget (script example)

Scripts receive no `init(sdk)` object, so this example uses `window.ChWebSdk`.

```javascript
window.ChWebSdk.loadStyle('https://example.com/widget.css')
window.ChWebSdk.loadScript('https://example.com/widget.js', () => {
  // Widget script is loaded
})
```

## Next Steps

* [User Context](user-context) — complete reference for `Context.User()`, all User methods, and the User object schema
* [Reference](methods-constructors) — DOM utilities, Content, User, Subscription methods, and error handling
* [Web SDK](overview) — how the SDK is loaded and when to use each API
