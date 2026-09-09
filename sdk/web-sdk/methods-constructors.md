---
url: >-
  https://developer-portal.gainsight.com/docs/sdk/web-sdk/methods-constructors.md
description: >-
  Complete API reference for the Community Hub Web SDK — DOM utilities, Content,
  User, Subscription methods, search options, and error handling.
---

#### Web SDK

# Methods and Constructors

Full reference for every method, option, and return type exposed by the `ChWebSdk` object on community pages.

## DOM and lifecycle

### `onReady(callback)`

Run a callback when the DOM is ready. If the document is already loaded (`complete` or `interactive`), the callback runs immediately; otherwise it runs on `DOMContentLoaded`.

```javascript
ChWebSdk.onReady(() => {
  // Safe to use DOM and SDK
})
```

### `observeElement(selector, callback)`

Wait until an element matching `selector` appears in the DOM, then run `callback` with that element. Uses a `MutationObserver` and disconnects after the element is found.

```javascript
ChWebSdk.observeElement('#my-widget-root', (el) => {
  el.textContent = 'Widget loaded'
})
```

### `onEvent(selector, event, handler, options?)`

Bind an event listener to the first element that matches `selector` (waiting for it via `observeElement` if needed). Returns an **unsubscribe** function to remove the listener.

| Parameter  | Type     | Description                                      |
|------------|----------|--------------------------------------------------|
| `selector` | `string` | CSS selector for the target element              |
| `event`    | `string` | DOM event name (e.g. `'click'`, `'submit'`)      |
| `handler`  | `function` | Event handler                                 |
| `options`  | `boolean \| AddEventListenerOptions` | Optional; e.g. `{ once: true }` |

```javascript
const unsubscribe = ChWebSdk.onEvent('.submit-btn', 'click', (ev) => {
  ev.preventDefault()
  // ...
})
// Later: unsubscribe() to remove the listener
```

### `loadScript(src, callback?)`

Load an external script by URL. If the script is already present, calls `callback` when appropriate. Scripts are loaded with `async: true`.

```javascript
ChWebSdk.loadScript('https://example.com/plugin.js', () => {
  console.log('Script loaded')
})
```

### `loadStyle(href)`

Inject a stylesheet link. No-op if a link with the same `href` already exists.

```javascript
ChWebSdk.loadStyle('https://example.com/widget.css')
```

***

## Content Methods

The Content Methods provide typed access to internal community content APIs (articles, questions, conversations, ideas, product updates), search, and actions like like, unlike, vote, and unvote.

### Content types and instances

Get a **content instance** for a specific type. Pass an optional **private topic ID** when you need to perform actions (like, unlike, vote, etc.) on a specific topic.

| Method            | Description                    |
|-------------------|--------------------------------|
| `Content.Article(privateId?)`  | Knowledge base articles       |
| `Content.Question(privateId?)` | Community questions           |
| `Content.Conversation(privateId?)` | Conversations / discussions |
| `Content.Idea(privateId?)`    | Ideation topics               |
| `Content.ProductUpdate(privateId?)` | Product updates          |

```javascript
// Search only ideas (no privateId needed)
const ideaInstance = ChWebSdk.Content.Idea()
const ideas = await ideaInstance.search('feature request', { limit: 10 })

// Like a specific idea (privateId required)
const idea = ChWebSdk.Content.Idea(12345)
await idea.like()  // uses current logged-in user
```

### Content instance methods

When created **with** a `privateId`, an instance supports:

| Method | Description |
|--------|-------------|
| `like(likedBy?)` | Like the topic. `likedBy` defaults to current user. |
| `unlike(unlikedBy?)` | Unlike the topic. |
| `vote(votedBy?)` | Vote (e.g. for ideas). |
| `unvote(unvotedBy?)` | Remove vote. |
| `likeReply(replyId, likedBy?)` | Like a reply. |
| `unlikeReply(replyId, unlikedBy?)` | Unlike a reply. |
| `search(query, options?)` | Search content of this type only. |

All of these require a **browser environment** and, where applicable, a **logged-in user** (or an explicit user ID).

### `Content.search(query, options?)`

Search **across all content types** (no content-type filter). Same options as the per-type search.

```javascript
const results = await ChWebSdk.Content.search('help with login', {
  limit: 20,
  page: 0,
  fetchMetadata: true
})
```

### Search options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `query` | `string` | — | Search query (required). |
| `contentType` | `string \| string[]` | — | Filter by type: `'idea'`, `'ideation'`, `'article'`, `'question'`, `'discussion'`, `'conversation'`, `'event'`, `'productUpdate'`. |
| `limit` | `number` | `30` | Max results per page. |
| `page` | `number` | `0` | Page index (0-based). |
| `fetchMetadata` | `boolean` | `true` | When `true`, enriches results with author, engagement, and topic metadata from the backend. |
| `categoryName` | `string` | — | Filter by community category name (top-level facet). |
| `categoryIds` | `number[]` | — | Filter by category/forum IDs. |
| `kbCategoryName` | `string` | — | Filter by knowledge base category name. |
| `productAreaName` | `string` | — | Filter by product area name. |
| `facetFilters` | `string[][]` | — | Raw facet filters for advanced filtering. |

### Search result shape

Each result includes at least: `id`, `title`, `url`, `contentType`, `excerpt`, and optionally `views`, `likes`, `votes`. When `fetchMetadata: true`, additional fields such as `firstPost`, `forum`, `numberOfViews`, `numberOfReplies`, `numberOfLikes`, `hasCurrentUserLiked`, `ideationStatus`, `bugStatus`, etc. are populated.

***

## User Methods

### `User.search(query)`

Search users by query string. Returns an array of user objects.

**Browser-only.** Requires a non-empty string `query`.

```javascript
const users = await ChWebSdk.User.search('john')
```

### `User.getUsersById(ids)`

Fetch users by an array of user IDs. Returns an object keyed by user ID.

**Browser-only.** Requires a non-empty array of IDs.

```javascript
const usersMap = await ChWebSdk.User.getUsersById(['1', '2', '3'])
const user1 = usersMap['1']
```

### User object shape

Each user includes: `id`, `url`, `name`, `avatar`, `userTitle`, `userLevel`, `badges`, `isBanned`, and optional `rank` (with `name`, `color`, `icon`, etc.).

***

## Subscription Methods

All subscription methods are **browser-only** and require valid identifiers.

### Topics

| Method | Description |
|--------|-------------|
| `Subscription.subscribeTopic(publicId)` | Subscribe to a topic by its **public** ID. |
| `Subscription.unsubscribeTopic(publicId)` | Unsubscribe from a topic. |
| `Subscription.getTopicSubscriptionStatus(publicId)` | Returns `true` if the current user is subscribed. |

### Categories

| Method | Description |
|--------|-------------|
| `Subscription.subscribeToCategory(categoryId)` | Subscribe to a category (ID as string). |
| `Subscription.unsubscribeFromCategory(categoryId)` | Unsubscribe from a category. |
| `Subscription.getCategorySubscriptionStatus(categoryId)` | Returns `true` if the current user is subscribed to the category. |

```javascript
await ChWebSdk.Subscription.subscribeTopic('topic-123')
const isSubscribed = await ChWebSdk.Subscription.getTopicSubscriptionStatus('topic-123')

await ChWebSdk.Subscription.subscribeToCategory('42')
const subscribedToCategory = await ChWebSdk.Subscription.getCategorySubscriptionStatus('42')
```

***

## Error handling

SDK methods that call the backend or DOM throw on failure. Use try/catch and handle missing or invalid arguments.

```javascript
try {
  const users = await ChWebSdk.User.search('john')
  console.log(users)
} catch (err) {
  console.error('User search failed:', err.message)
}
```

Common cases:

| Scenario | Cause |
|----------|-------|
| "can only be called in a browser environment" | Method was run in Node or an environment without `window`. |
| "Search query is required" / "User IDs array is required" | Missing or invalid arguments. |
| "privateId is required for this operation" | Content action (like, vote, etc.) was called on an instance created without a private ID. |
| "User must be logged in or provide a userId" | Like/vote/unvote used without a logged-in user or explicit ID. |
| HTTP/network errors | Backend request failed (e.g. subscription, search, user fetch). |

***

## Next Steps

* [Examples](examples) — runnable code covering search, subscriptions, users, and DOM interactions
* [Web SDK](overview) — how the SDK is loaded and when to use each method.
