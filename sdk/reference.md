---
url: https://developer-portal.gainsight.com/docs/sdk/reference.md
description: >-
  Complete reference for the SDK — the connectors and web clients imported from
  @gainsight-hub/sdk, and the per-widget runtime on the init(sdk) object
---

# SDK Reference

Every method, option, and event in the SDK: the [`connectors`](#connectors) and [`web`](#web) clients imported from `@gainsight-hub/sdk`, and the [`init(sdk)` runtime](#init-sdk-runtime) passed to each widget. See [SDK](/sdk/) for how the import resolves and how to set up your project.

## `connectors`

`connectors` calls [connectors](/connectors/) — the backend proxies that reach external APIs so credentials stay off the browser. It is one client shared by every widget on the page: per-request options such as `headers` affect only that call, but [`connectors.configure(...)`](#connectors-configure-config) changes the client for all widgets on the page.

### Import

```javascript
import { connectors } from "@gainsight-hub/sdk";
```

### `connectors.configure(config)`

Change client settings. The settings apply to every request made through `connectors`, across all widgets on the page. All options are optional.

```javascript
connectors.configure({
  timeout: 15000,
  headers: { "X-Custom-Header": "value" }
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `headers` | `Record<string, string>` | `{}` | Headers included with every request, merged with any set earlier |
| `timeout` | `number` | `30000` | Request timeout in milliseconds |
| `csrfToken` | `string` | read from the page | CSRF token sent with every request |

### `connectors.execute(options)`

Execute a connector and return the response.

```javascript
const result = await connectors.execute({
  permalink: "my-connector",
  method: "POST",
  payload: { name: "Monstera", size: "M" },
  headers: { "X-Request-Id": "abc-123" },
  queryParams: { format: "json" },
  pathParams: { user_id: 42 }
});
```

**Parameters**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `permalink` | `string` | Yes | — | The connector's unique permalink identifier |
| `method` | `"GET" \| "POST" \| "PUT" \| "PATCH" \| "DELETE" \| "HEAD" \| "OPTIONS"` | Yes | — | HTTP method |
| `payload` | `Record<string, JsonValue>` | No | `undefined` | The request body, as a plain JSON object. Pass it via `payload` (not `body`). Only sent for `POST`, `PUT`, `PATCH` methods |
| `queryParams` | `Record<string, string>` | No | `{}` | Query parameters appended to the request URL |
| `pathParams` | `Record<string, string \| number>` | No | `{}` | Values substituted into {{ pathParams.X }} URL template references on the connector. See [Dynamic URL Path Segments](/connectors/dynamic-url-paths) |
| `headers` | `Record<string, string>` | No | `{}` | Additional headers for this request |

`JsonValue` covers strings, numbers, booleans, null, arrays, and nested objects.

::: warning Pass the request body via `payload`, not `body`
The request body goes in the `payload` option as a plain JSON object. `body` is not a recognized option — and `execute()` silently ignores any option it does not recognize, so a call using `body` sends an empty request body.
:::

**Returns** `Promise<Record<string, JsonValue>>`

#### Path parameter validation

`pathParams` is validated by the SDK before any network call. The returned promise rejects, and no request is sent, on:

* An empty or whitespace-only key (e.g. `{ "": "x" }`)
* A value whose type is not `string | number` — catches untyped JS callers passing `null`, `undefined`, booleans, objects, or arrays
* A serialized header value over 4096 characters (gateway / CDN per-header limits typically sit around 8–16 KB; this cap leaves headroom)
* Passing `pathParams` to `connectors.composite.execute` (composite connectors don't accept path parameters)

Omitting `pathParams` or passing `{}` sends no header — the request goes out unchanged. See [Dynamic URL Path Segments — Validation and errors](/connectors/dynamic-url-paths#validation-and-errors) for the server-side error codes returned when validation passes the SDK but fails server-side.

### Connector errors

`execute` returns a promise that rejects when the call fails — a network error, a timeout, an invalid `pathParams` value, or an error response from the connector or the API behind it. Wrap calls in `try`/`catch` and read `error.message`:

```javascript
try {
  const data = await connectors.execute({
    permalink: "my-connector",
    method: "GET",
    pathParams: { user_id: 42 }
  });
  console.log(data);
} catch (error) {
  console.error("Connector request failed:", error.message);
  // show a fallback in your widget
}
```

For path-parameter errors returned by the server, `error.code` names the problem (`MISSING_PATH_PARAMETERS` or `INVALID_PATH_PARAMETERS`) — see [Dynamic URL Path Segments](/connectors/dynamic-url-paths#validation-and-errors).

## `web`

`web` searches content, manages users, and subscribes to topics and categories on community pages. It works in widgets and in sitewide scripts registered with `"type": "module"`, forwards to the community's Web SDK, and throws on pages that do not have it.

`web` is **browser-only**: all methods that call the backend throw if run outside a browser environment.

### DOM and lifecycle

#### `loadScript(src, callback?)`

Load an external script by URL. If the script is already present, calls `callback` when appropriate. Scripts are loaded with `async: true`.

```javascript
import { web } from "@gainsight-hub/sdk";

web.loadScript('https://example.com/plugin.js', () => {
  console.log('Script loaded')
})
```

***

### Content Methods

The Content Methods provide typed access to internal community content APIs (articles, questions, conversations, ideas, product updates), search, and actions like like, unlike, vote, and unvote.

#### Content types and instances

Get a **content instance** for a specific type. Pass an optional **private topic ID** when you need to perform actions (like, unlike, vote, etc.) on a specific topic.

| Method            | Description                    |
|-------------------|--------------------------------|
| `Content.Article(privateId?)`  | Knowledge base articles       |
| `Content.Question(privateId?)` | Community questions           |
| `Content.Conversation(privateId?)` | Conversations / discussions |
| `Content.Idea(privateId?)`    | Ideation topics               |
| `Content.ProductUpdate(privateId?)` | Product updates          |

```javascript
import { web } from "@gainsight-hub/sdk";

// Search only ideas (no privateId needed)
const ideaInstance = web.Content.Idea()
const ideas = await ideaInstance.search('feature request', { limit: 10 })

// Like a specific idea (privateId required)
const idea = web.Content.Idea(12345)
await idea.like()  // uses current logged-in user
```

#### Content instance methods

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

#### `Content.search(query, options?)`

Search **across all content types** (no content-type filter). Same options as the per-type search.

```javascript
import { web } from "@gainsight-hub/sdk";

const results = await web.Content.search('help with login', {
  limit: 20,
  page: 0,
  fetchMetadata: true
})
```

#### Search options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `query` | `string` | — | Search query (required). Must be a non-empty string; an empty string is rejected. |
| `contentType` | `string \| string[]` | — | Filter by type: `'idea'`, `'ideation'`, `'article'`, `'question'`, `'discussion'`, `'conversation'`, `'event'`, `'productUpdate'`. |
| `limit` | `number` | `30` | Max results per page. |
| `page` | `number` | `0` | Page index (0-based). |
| `fetchMetadata` | `boolean` | `true` | When `true`, enriches results with author, engagement, and topic metadata from the backend. |
| `categoryName` | `string` | — | Filter by community category name (top-level category). |
| `categoryIds` | `number[]` | — | Filter by category/forum IDs. |
| `kbCategoryName` | `string` | — | Filter by knowledge base category name. |
| `productAreaName` | `string` | — | Filter by product area name. |

Search has no sort option and no date filter. Results are returned in relevance order, so none of the options above can list the newest, most viewed, most replied, or most liked content. The filters only narrow the set of results.

Only content the current visitor can see is returned, whatever filters you set.

#### Search result shape

Each result includes at least: `id`, `title`, `url`, `contentType`, `excerpt`, and optionally `views`, `likes`, `votes`. When `fetchMetadata: true`, additional fields such as `firstPost`, `forum`, `numberOfViews`, `numberOfReplies`, `numberOfLikes`, `hasCurrentUserLiked`, `ideationStatus`, `bugStatus`, etc. are populated. `firstPost` carries `author` (an object with `name`), `creationDate` (ISO 8601 timestamp) and `relativeCreationDate` (for example "1 year ago").

***

### Context Methods

Access the current session context — who is viewing the page and their community profile.

#### `Context.User()`

Fetch the current user. Always returns a `User` object — a guest resolves to the **guest projection** (`userId: null`, `username: "guest"`), never `null`. This replaces reading `inSidedData.user`.

**Browser-only.** Takes no parameters. For a signed-in visitor `badges` and `profileFields` are always present and `rank` is present when the user has one; the guest result has no `profileFields` or `rank`. No private account data (email, PM counts, login source) is ever returned. The response does include the viewer's own private profile fields, which any script on the page can read; see [User Context](/sdk/user-context#context-current-viewer).

```javascript
import { web } from "@gainsight-hub/sdk";

const me = await web.Context.User()
if (me.userId === null) {
  // Guest — show login prompt
} else {
  console.log(me.username, me.rank?.name, me.badges)
}
```

See [User Context](/sdk/user-context) for the complete reference — full User object schema, guest handling, field availability by method, and all examples.

***

### User Methods

All User methods are **browser-only**.

#### `User.search(query)`

Search users by a query string. Returns a reduced field set optimized for mention/autocomplete UIs — use `User.list()` when you need the full user object.

Requires a non-empty string `query`. Returns **up to 25 users** — this limit is fixed and there is no size parameter.

```javascript
import { web } from "@gainsight-hub/sdk";

const results = await web.User.search('john')
results.forEach((u) => console.log(u.userId, u.username, u.avatar))
```

#### `User.getUsersById(ids)`

Batch-fetch up to 100 users by ID. Unresolved IDs are silently omitted — the result array may be shorter than the input.

Numeric strings are accepted and coerced. Returns an array (not a map keyed by ID — iterate with `.find()` or build your own map).

```javascript
import { web } from "@gainsight-hub/sdk";

const users = await web.User.getUsersById([101, 202, 303])
const user = users.find((u) => u.userId === 101)
```

#### `User.getById(id)`

Fetch a single user by ID. Returns `null` if the ID does not resolve.

```javascript
import { web } from "@gainsight-hub/sdk";

const user = await web.User.getById(101)
if (user) console.log(user.username)
```

#### `User.list(options?)`

List and filter community members with pagination and sorting. Returns `{ totalItems, items }` — **defaults to 25 items per page (max 100)**.

```javascript
import { web } from "@gainsight-hub/sdk";

const { totalItems, items } = await web.User.list({
  role: [7],
  sort: 'lastVisit',
  order: 'desc',
  size: 20
})
```

Key options: `search`, `role`, `joinDate`, `lastActivity`, `topics`, `replies`, `points`, `page`, `size`, `sort`, `order`. See [User Context](/sdk/user-context#user-lookup-and-listing-methods) for the full options reference.

#### `User.getRecentlyActive(limit?)`

Returns users sorted by last visit descending. Convenience wrapper around `list()`.

```javascript
import { web } from "@gainsight-hub/sdk";

const users = await web.User.getRecentlyActive(10)
```

#### `User.getByRole(role, options?)`

List users that hold one or more roles. `role` is a single role ID or an array (OR semantics).

```javascript
import { web } from "@gainsight-hub/sdk";

const { items } = await web.User.getByRole(7)
const { items } = await web.User.getByRole([7, 9], { size: 50 })
```

#### User object shape

All User methods except `search()` return the unified `User` model; `search()` returns a reduced `UserSearchResult`. A field with no value is omitted, not `null`, so read optional fields with `?.`. See [User Context](/sdk/user-context#user-object-shape) for every field and its type, and [Field Availability by Method](/sdk/user-context#field-availability-by-method) for which methods return which fields.

***

### Subscription Methods

All subscription methods are **browser-only** and require valid identifiers.

#### Topics

| Method | Description |
|--------|-------------|
| `Subscription.subscribeToTopic(publicId)` | Subscribe to a topic by its **public** ID. |
| `Subscription.unsubscribeFromTopic(publicId)` | Unsubscribe from a topic. |
| `Subscription.getTopicSubscriptionStatus(publicId)` | Returns `true` if the current user is subscribed. |

#### Categories

| Method | Description |
|--------|-------------|
| `Subscription.subscribeToCategory(categoryId)` | Subscribe to a category (ID as string). |
| `Subscription.unsubscribeFromCategory(categoryId)` | Unsubscribe from a category. |
| `Subscription.getCategorySubscriptionStatus(categoryId)` | Returns `true` if the current user is subscribed to the category. |

```javascript
import { web } from "@gainsight-hub/sdk";

await web.Subscription.subscribeToTopic('topic-123')
const isSubscribed = await web.Subscription.getTopicSubscriptionStatus('topic-123')

await web.Subscription.subscribeToCategory('42')
const subscribedToCategory = await web.Subscription.getCategorySubscriptionStatus('42')
```

***

### Web SDK errors

SDK methods that call the backend or DOM throw on failure. Use try/catch and handle missing or invalid arguments.

```javascript
import { web } from "@gainsight-hub/sdk";

try {
  const users = await web.User.search('john')
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

## `init(sdk)` runtime

The `sdk` instance the platform passes to your widget's `init(sdk)` function. It is the per-widget runtime: shadow root, props, design tokens, and events. `connectors` and `web` are not on this object; import them from `@gainsight-hub/sdk`.

### Properties

| Property      | Type         | Description                             |
| ------------- | ------------ | --------------------------------------- |
| `root`        | `ShadowRoot` | The widget's shadow root (mount target) |
| `shadowRoot`  | `ShadowRoot` | Alias for `root` — the same shadow root reference |
| `document`    | `Document`   | Global document reference               |
| `version`     | `string`     | SDK version (e.g. `"1.5.2"`)           |

### Methods

| Method               | Returns          | Description                                    |
| -------------------- | ---------------- | ---------------------------------------------- |
| `getContainer()`     | `ShadowRoot`     | Returns the widget's shadow root — same reference as `root`/`shadowRoot`, recommended mount target for framework apps (React, Vue, etc.) |
| `getProps<T>()`      | `T`              | Current widget props (from configuration)      |
| `setProps(props)`    | `void`           | Merges `props` into the current props (keys are never removed) and emits `propsChanged`. Local to this widget instance; not persisted.|
| `getVersionInfo()`   | `VersionInfo`    | `{ version, gitSha, buildTime }`               |
| `getDesignTokens()`  | `Record<string, string \| null>` | Current theme design tokens, keyed by token name without the leading `--` (e.g. `config--main-color-brand` for `--config--main-color-brand`). Returns a copy |
| `whenReady()`        | `Promise<SDK>`   | Resolves when the widget is ready to use. Await it as the first statement of `init`.|
| `$(selector)`        | `Element \| null` | Shorthand for `shadowRoot.querySelector(selector)` |
| `$$(selector)`       | `Element[]`      | Shorthand for `shadowRoot.querySelectorAll(selector)` — returns an array, not a NodeList |
| `on(event, handler)` | `() => void`     | Subscribe to an event (returns unsubscribe fn) |
| `off(event, handler)`| `void`           | Unsubscribe from an event                      |
| `emit(event, data?)` | `void`           | Emit an event to this widget's own `on()` listeners|

### Built-in Events

| Event                 | Payload                              | Trigger                              |
| --------------------- | ------------------------------------ | ------------------------------------ |
| `propsChanged`        | `Record<string, unknown>`            | Widget configuration changes         |
| `destroy`             | `undefined`                          | Widget removed from DOM              |
| `error`               | `{ message: string, error?: Error }` | Module load or initialization error  |
| `designTokensChanged` | `Record<string, string \| null>`     | Theme design tokens change while the page is open, e.g. live edits in the theme configurator. Payload is the full token map, same shape as `getDesignTokens()` |

## Next Steps

* [SDK Examples](/sdk/examples) — runnable code for connectors, search, subscriptions, and users
* [User Context](/sdk/user-context) — complete reference for `Context.User()`, all User methods, and the User object schema
* [Calling from Widget Code](/connectors/calling-from-widgets) — how to call connectors from a widget
* [Widget Runtime](/custom-widgets/v2/core-concepts) — runtime model, lifecycle, props, and design tokens
* [Configurable Widgets](/custom-widgets/v2/configurable-widgets) — let editors customize your widget via a form in the **No-Code Builder**
* [Using React](/custom-widgets/v2/using-react) — build widgets with React
* [Rendering & DOM](/custom-widgets/v2/rendering-and-dom) — Shadow DOM context for DOM manipulation
