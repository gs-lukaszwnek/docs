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
  await ChWebSdk.Subscription.subscribeToTopic('topic-abc')
  const isSubscribed = await ChWebSdk.Subscription.getTopicSubscriptionStatus('topic-abc')
  console.log(isSubscribed) // true
})
```

## Get the current user (viewer context)

```javascript
ChWebSdk.onReady(async () => {
  const me = await ChWebSdk.Context.User()

  if (me.userId === null) {
    // Guest (guest projection) — show login prompt or public-only content
    console.log('Not logged in')
    return
  }

  // Authenticated viewer
  console.log(me.username)     // 'janedoe'
  console.log(me.userId)       // 101
  console.log(me.rank?.name)   // 'Regular'
  console.log(me.badges)       // [{ id: 100, title: 'First post', url: '/badge/first-post' }]
})
```

## Show the current user's badges

```javascript
ChWebSdk.onReady(async () => {
  const me = await ChWebSdk.Context.User()
  if (me.userId === null || me.badges.length === 0) return

  const list = document.createElement('ul')
  me.badges.forEach((badge) => {
    const li = document.createElement('li')
    li.textContent = badge.title
    list.appendChild(li)
  })
  document.getElementById('badges')?.appendChild(list)
})
```

## Personalize based on profile fields

```javascript
ChWebSdk.onReady(async () => {
  const me = await ChWebSdk.Context.User()
  if (me.userId === null) return

  // Find a profile field by ID (field IDs are specific to your community)
  const seniority = me.profileFields.find((f) => f.id === 3)
  if (seniority?.value === 'senior') {
    document.getElementById('advanced-content')?.removeAttribute('hidden')
  }
})
```

## Find users and use in UI

```javascript
ChWebSdk.onReady(async () => {
  const users = await ChWebSdk.User.getUsersById([1, 2, 3])
  users.forEach((u) => console.log(u.userId, u.username, u.avatar))
})
```

## List moderators

```javascript
ChWebSdk.onReady(async () => {
  const { totalItems, items } = await ChWebSdk.User.list({
    role: [7], // role IDs are specific to your community
    sort: 'lastVisit',
    order: 'desc',
    size: 10
  })
  console.log(`${items.length} of ${totalItems} moderators`)
  items.forEach((u) => console.log(u.username, u.rank?.name))
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

* [User Context](user-context) — complete reference for `Context.User()`, all User methods, and the User object schema
* [Methods and Constructors](methods-constructors) — DOM utilities, Content, User, Subscription methods, and error handling
* [Web SDK](overview) — how the SDK is loaded and when to use each API
