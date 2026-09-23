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

## Context Methods

Access the current session context — who is viewing the page and their public community profile.

### `Context.User()`

Fetch the current user. Always returns a `User` object — a guest resolves to the **guest projection** (`userId: null`, `username: "guest"`), never `null`. This replaces reading `inSidedData.user`.

**Browser-only.** Takes no parameters. `rank`, `badges`, and `profileFields` are always hydrated. No private data (email, PM counts, login source) is ever returned — not even for the authenticated viewer.

```javascript
ChWebSdk.onReady(async () => {
  const me = await ChWebSdk.Context.User()
  if (me.userId === null) {
    // Guest — show login prompt
    return
  }
  console.log(me.username, me.rank?.name, me.badges)
})
```

See [User Context](user-context) for the complete reference — full User object schema, guest projection, field availability by method, and all examples.

***

## User Methods

All User methods are **browser-only**.

### `User.search(query)`

Search users by a query string. Returns a reduced field set optimized for mention/autocomplete UIs — use `User.list()` when you need the full user object.

Requires a non-empty string `query`. Returns **up to 25 users** — this limit is fixed and there is no size parameter.

```javascript
const results = await ChWebSdk.User.search('john')
results.forEach((u) => console.log(u.userId, u.username, u.avatar))
```

### `User.getUsersById(ids)`

Batch-fetch up to 100 users by ID. Unresolved IDs are silently omitted — the result array may be shorter than the input.

Numeric strings are accepted and coerced. Returns an array (not a map keyed by ID — iterate with `.find()` or build your own map).

```javascript
const users = await ChWebSdk.User.getUsersById([101, 202, 303])
const user = users.find((u) => u.userId === 101)
```

### `User.getById(id)`

Fetch a single user by ID. Returns `null` if the ID does not resolve.

```javascript
const user = await ChWebSdk.User.getById(101)
if (user) console.log(user.username)
```

### `User.list(options?)`

List and filter community members with pagination and sorting. Returns `{ totalItems, items }` — **defaults to 25 items per page (max 100)**.

```javascript
const { totalItems, items } = await ChWebSdk.User.list({
  role: [7],
  sort: 'lastVisit',
  order: 'desc',
  size: 20
})
```

Key options: `search`, `role`, `joinDate`, `lastActivity`, `topics`, `replies`, `points`, `page`, `size`, `sort`, `order`. See [User Context](user-context#user-lookup-and-listing-methods) for the full options reference.

### `User.getRecentlyActive(limit?)`

Returns users sorted by last visit descending. Convenience wrapper around `list()`.

```javascript
const users = await ChWebSdk.User.getRecentlyActive(10)
```

### `User.getByRole(role, options?)`

List users that hold one or more roles. `role` is a single role ID or an array (OR semantics).

```javascript
const { items } = await ChWebSdk.User.getByRole(7)
const { items } = await ChWebSdk.User.getByRole([7, 9], { size: 50 })
```

### User object shape

All User methods return objects conforming to the unified `User` model. Key fields:

| Field | Type | Notes |
|---|---|---|
| `userId` | `number \| null` | Canonical ID. Use this in new code — `id` is a deprecated alias. |
| `username` | `string` | Display username. |
| `avatar` | `string` | Avatar URL, or `""` when none. |
| `profileUrl` | `string \| null` | Relative URL to the profile page. |
| `rank` | `Rank \| null` | Hydrated rank with display styling (always present). |
| `badges` | `Badge[]` | Hydrated badges (always present). |
| `profileFields` | `ProfileField[]` | Custom profile fields (always present). |
| `isBanned` | `boolean` | |
| `isModerator` | `boolean` | |
| `mainRole` | `string \| null` | Role slug (e.g. `"moderator"`). |
| `reputation` | `number \| null` | Reputation score. |
| `topics`, `replies`, `points` | `number` | Activity counts. |
| `joinDate` | `string` | ISO-8601 datetime (not a Unix timestamp). |

See [User Context](user-context#user-object-shape) for every field, type, and availability table.

***

## Subscription Methods

All subscription methods are **browser-only** and require valid identifiers.

### Topics

| Method | Description |
|--------|-------------|
| `Subscription.subscribeToTopic(publicId)` | Subscribe to a topic by its **public** ID. |
| `Subscription.unsubscribeFromTopic(publicId)` | Unsubscribe from a topic. |
| `Subscription.getTopicSubscriptionStatus(publicId)` | Returns `true` if the current user is subscribed. |

### Categories

| Method | Description |
|--------|-------------|
| `Subscription.subscribeToCategory(categoryId)` | Subscribe to a category (ID as string). |
| `Subscription.unsubscribeFromCategory(categoryId)` | Unsubscribe from a category. |
| `Subscription.getCategorySubscriptionStatus(categoryId)` | Returns `true` if the current user is subscribed to the category. |

```javascript
await ChWebSdk.Subscription.subscribeToTopic('topic-123')
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

* [User Context](user-context) — complete reference for `Context.User()`, all User methods, User object schema, and examples
* [Examples](examples) — runnable code covering search, subscriptions, users, and DOM interactions
* [Web SDK](overview) — how the SDK is loaded and when to use each method.
