---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/user-context.md
description: >-
  Complete reference for user context in the ChWebSdk — fetching the current
  viewer, looking up community members, the full User object schema, guest
  handling, and field availability by method.
---

# User Context

This page documents the `ChWebSdk` user-context API: fetching the current viewer with `ChWebSdk.Context.User()` and looking up community members with the `ChWebSdk.User.*` methods. All run as the visiting user, authorized by the community session cookie.

::: tip
Prefer these SDK methods over a Connector for user data — they use the browser session and need no server-side secrets.
:::

## Context: Current Viewer

### `Context.User()`

Fetches the current user for the active session. This is the direct replacement for reading `inSidedData.user`.

**Returns:** `Promise<User>`

* Resolves to a `User` object for an authenticated visitor.
* For an unauthenticated (guest) visitor, resolves to the **guest projection** — a `User` object with `userId: null` and `username: "guest"`. It is **never `null`**; branch on `userId` to tell guest from member.
* Throws if called outside a browser environment or if the network request fails.

Takes no parameters. `rank`, `badges`, and `profileFields` are always hydrated on every response.

::: warning No private data on the current viewer
`Context.User()` is readable by any script on the community page, so it never exposes private data — **even for the authenticated viewer**. Email address, private-message counts, subscription count, and login/registration source are not included in the response.
:::

**Checking authenticated vs. guest:** branch on `userId`, not on `null`.

```javascript
ChWebSdk.onReady(async () => {
  const me = await ChWebSdk.Context.User()

  if (me.userId === null) {
    // Guest — show a login prompt or public-only content
    return
  }

  // Authenticated — personalize the UI
  console.log(me.username)        // 'janedoe'
  console.log(me.userId)          // 101
  console.log(me.rank?.name)      // 'Regular'
  console.log(me.badges)          // [{ id: 100, title: 'First post', url: '/badge/first-post' }]
  console.log(me.profileFields)   // [{ id: 3, title: 'Seniority', value: 'senior', ... }]
})
```

***

## User: Lookup and Listing Methods

All methods below are **browser-only** and operate on the community user directory. They return the public `User` shape — no private data (email, private-message counts, login source) is exposed by any of them.

### `User.getById(id)`

Fetch a single user by numeric ID.

**Returns:** `Promise<User | null>` — resolves to `null` if the ID does not resolve.

| Parameter | Type | Description |
|---|---|---|
| `id` | `number \| string` | User ID. Numeric strings are accepted and coerced. |

```javascript
const user = await ChWebSdk.User.getById(101)
if (user) {
  console.log(user.username, user.rank?.name)
}
```

### `User.getUsersById(ids)`

Fetch multiple users by an array of IDs in one request (batch lookup). Unresolved IDs are silently omitted — the result array may be shorter than the input.

Up to 100 IDs per request. IDs beyond the hundredth are dropped, not refused.

**Returns:** `Promise<User[]>` — array of resolved users. Order is **not guaranteed** to match the input; match results by `userId` (e.g. with `.find()`), not by position. Each item is a `User` object.

| Parameter | Type | Description |
|---|---|---|
| `ids` | `Array<number \| string>` | User IDs to fetch. Numeric strings are accepted. |

```javascript
const users = await ChWebSdk.User.getUsersById([101, 202, 303])
// users.length may be less than 3 if any ID did not resolve
users.forEach((u) => console.log(u.userId, u.username))
```

### `User.list(options?)`

List and filter community members with pagination, sorting, and filtering. Returns a paginated result.

**Returns:** `Promise<{ totalItems: number, items: User[] }>`

* `totalItems`: total number of users matching the filters (before pagination).
* `items`: users on the requested page. **Defaults to 25 per page (max 100)** when `size` is omitted.
* Every row is a fully hydrated `User` (`rank`, `badges`, `profileFields` included), so a full page of 100 is a heavier payload — request only the `size` you need.

| Option | Type | Default | Description |
|---|---|---|---|
| `search` | `string` | — | Substring match on username. An authenticated caller may also pass an email address here; a guest who does gets a 403 (guests can still search by username). |
| `role` | `number[]` | — | Filter by main role ID(s). OR semantics. |
| `joinDate` | `DateFilter` | — | Filter on registration date. |
| `lastActivity` | `DateFilter` | — | Filter on last activity date. |
| `lastVisit` | `DateFilter` | — | Filter on last visit (login) date. |
| `topics` | `NumberFilter` | — | Filter on topics created. |
| `replies` | `NumberFilter` | — | Filter on replies created. |
| `points` | `NumberFilter` | — | Filter on points. |
| `page` | `number` | `1` | 1-indexed page number. |
| `size` | `number` | `25` | Page size (1–100). |
| `sort` | `UserSortField` | `'userId'` | Sort field. |
| `order` | `'asc' \| 'desc'` | `'desc'` | Sort direction. |

Role, badge, and profile-field IDs used in the filters and examples below are **specific to each community**. Find your community's values in its admin settings (the Roles, Badges, and Profile Fields sections) instead of reusing the sample IDs.

**`DateFilter`** — ISO-8601 datetime, ISO-8601 duration relative to now (e.g. `'P3D'` = last 72 h), or `{ from?, to? }` range:

```javascript
// Users who joined in the last 7 days
const { items } = await ChWebSdk.User.list({ joinDate: 'P7D' })

// Users who joined in a specific range
const { items } = await ChWebSdk.User.list({
  joinDate: { from: '2024-01-01T00:00:00Z', to: '2024-12-31T23:59:59Z' }
})
```

**`NumberFilter`** — exact integer or `{ eq?, gt?, gte?, lt?, lte? }` range:

```javascript
// Users with 10 or more topics
const { items } = await ChWebSdk.User.list({ topics: { gte: 10 } })
```

**`UserSortField`** values: `'userId'` | `'username'` | `'replies'` | `'topics'` | `'points'` | `'lastVisit'` | `'joinDate'` | `'lastActivity'`

**Example — list moderators sorted by last visit:**

```javascript
ChWebSdk.onReady(async () => {
  const { totalItems, items } = await ChWebSdk.User.list({
    role: [7],
    sort: 'lastVisit',
    order: 'desc',
    size: 20
  })
  console.log(`${items.length} of ${totalItems} moderators`)
  items.forEach((u) => console.log(u.username, u.lastVisit))
})
```

**Example — paginate through all users:**

```javascript
ChWebSdk.onReady(async () => {
  const size = 100
  let page = 1
  let total = Infinity

  while ((page - 1) * size < total) {
    const result = await ChWebSdk.User.list({ page, size })
    total = result.totalItems
    result.items.forEach((u) => console.log(u.userId, u.username))
    page++
  }
})
```

### `User.getRecentlyActive(limit?)`

Convenience wrapper around `list()`. Returns users sorted by `lastVisit` descending.

**Returns:** `Promise<User[]>`

| Parameter | Type | Default | Description |
|---|---|---|---|
| `limit` | `number` | `10` | Maximum users to return. |

```javascript
const recentUsers = await ChWebSdk.User.getRecentlyActive(5)
recentUsers.forEach((u) => console.log(u.username, u.lastVisit))
```

### `User.getByRole(role, options?)`

List users that hold one or more specified roles.

**Returns:** `Promise<{ totalItems: number, items: User[] }>` — **defaults to 25 per page (max 100)**, same as `list()`.

| Parameter | Type | Description |
|---|---|---|
| `role` | `number \| number[]` | Role ID or array of IDs. OR semantics. |
| `options` | `object` | Same pagination and sort options as `list()` (minus `role`). |

```javascript
// Users with role id 7, first page
const { totalItems, items } = await ChWebSdk.User.getByRole(7, { size: 50 })

// Users with any of these roles
const { items } = await ChWebSdk.User.getByRole([7, 9])
```

### `User.search(query)`

Search users by a query string. Returns a reduced field set optimized for mention/autocomplete UIs — use `User.list()` when you need the full user object.

**Requires an authenticated session** — a guest calling `search()` receives a 403 (this is a different endpoint from `list()`, and it is not open to guests at all).

**Returns:** `Promise<UserSearchResult[]>` — **up to 25 users**. This limit is fixed; `search()` takes no size parameter.

| Parameter | Type | Description |
|---|---|---|
| `query` | `string` | Non-empty search query |

**`UserSearchResult` object shape:**

| Field | Type | Notes |
|---|---|---|
| `userId` | `number` | Canonical user ID. |
| `username` | `string` | Display username. |
| `profileUrl` | `string \| null` | Relative URL to the profile page. |
| `avatar` | `string` | Avatar URL, or `""` when none. |
| `userTitle` | `string` | Display title. |
| `reputation` | `number \| null` | Reputation score. |
| `isBanned` | `boolean` | `true` if the user holds the banned role. |
| `badges` | `Badge[]` | Hydrated badges, **without `id`**. |
| `rank` | `Rank \| null` | Hydrated rank, **without `id`**. |

```javascript
const results = await ChWebSdk.User.search('jane')
results.forEach((u) => console.log(u.userId, u.username, u.avatar))
```

***

## User Object Shape

All methods above return objects conforming to the unified `User` model.

### Core identity fields

| Field | Type | Notes |
|---|---|---|
| `userId` | `number \| null` | Canonical user ID. `null` only on the guest projection. Use this in new code. |
| `id` | `number \| null` | **Deprecated** mirror of `userId`. Kept for backward compatibility; migrate to `userId`. |
| `username` | `string` | Display username. `"guest"` on the guest projection. |
| `name` | `string` | **Deprecated** alias of `username`. |
| `firstName` | `string \| null` | Often `null` due to visibility settings. |
| `lastName` | `string \| null` | |
| `avatar` | `string` | Avatar URL, or empty string `""` when none. |
| `profileUrl` | `string \| null` | Relative URL to the user's profile page (e.g. `"/user/janedoe"`). |
| `signature` | `string \| null` | User signature. |
| `userTitle` | `string` | Display title (custom title or rank name; empty string if hidden). |
| `companyId` | `string \| null` | Company identifier. |

### Role and moderation fields

| Field | Type | Notes |
|---|---|---|
| `isBanned` | `boolean` | `true` if the user holds the banned role. |
| `isModerator` | `boolean` | `true` if the user holds any moderator role. |
| `role` | `number \| null` | Main role ID as an integer. |
| `mainRole` | `string \| null` | Main role slug (e.g. `"moderator"`, `"roles.guest"`). |
| `customRoles` | `number[]` | Custom role IDs. |

### Rank fields

| Field | Type | Notes |
|---|---|---|
| `rankId` | `number \| null` | Rank ID. `null` when the user has no rank. |
| `rank` | `Rank \| null` | Hydrated rank with display styling. The property is always present; the value is `null` when the user has no rank. |

**`Rank` object shape:**

| Field | Type | Notes |
|---|---|---|
| `id` | `number` | Rank ID |
| `name` | `string` | Rank display name (e.g. `"Regular"`) |
| `color` | `string \| null` | Hex color for the rank name (e.g. `"#3366ff"`) |
| `isBold` | `boolean` | Whether the rank name should be bold |
| `isItalic` | `boolean` | Whether the rank name should be italic |
| `isUnderline` | `boolean` | Whether the rank name should be underlined |
| `icon` | `string \| null` | Icon identifier |
| `iconUrl` | `string \| null` | URL to the rank icon image |
| `avatarIcon` | `string \| null` | Avatar icon identifier |
| `avatarIconUrl` | `string \| null` | URL to the avatar icon image |

### Badge fields

| Field | Type | Notes |
|---|---|---|
| `badges` | `Badge[]` | Hydrated badges. Always present. Unresolvable badge IDs are dropped — the array only contains objects. |

**`Badge` object shape:**

| Field | Type | Notes |
|---|---|---|
| `id` | `number` | Badge ID |
| `title` | `string` | Badge display name (e.g. `"First post"`) |
| `url` | `string \| null` | Relative URL to the badge page (e.g. `"/badge/first-post"`) |

### Group and profile fields

| Field | Type | Notes |
|---|---|---|
| `groups` | `number[]` | Group IDs the user belongs to. |
| `profileFields` | `ProfileField[]` | Structured custom profile fields. Always hydrated. |

**`ProfileField` object shape:**

| Field | Type | Notes |
|---|---|---|
| `id` | `number` | Profile field ID |
| `title` | `string` | Profile field label (e.g. `"Seniority"`) |
| `type` | `string` | Field type: `'text'`, `'textarea'`, `'select'`, `'radio'`, `'check'`, `'multiselect'`, `'date'`, etc. |
| `value` | `string \| number \| boolean \| string[] \| object \| null` | The user's value for this field. `null` when unset. |
| `visibility` | `number` | Visibility level of the field, as an integer. |

### Activity and engagement fields

| Field | Type | Notes |
|---|---|---|
| `topics` | `number` | Topics created. |
| `replies` | `number` | Replies created. |
| `points` | `number` | Points. |
| `solved` | `number` | Best-answer / solved count. |
| `reputation` | `number \| null` | Reputation score. `null` if the reputation feature is disabled. |
| `likesReceived` | `number` | Likes received. |
| `likesGiven` | `number` | Likes given. |
| `followers` | `number` | Follower count. |
| `following` | `number` | Following count. |

### Date fields

| Field | Type | Notes |
|---|---|---|
| `joinDate` | `string` | Registration timestamp as ISO-8601 datetime (e.g. `"2022-04-12T09:23:18Z"`). **Not** a Unix timestamp — the legacy `inSidedData.user.joindate` was a Unix int; this is a string. |
| `lastActivity` | `string \| null` | Last activity timestamp (ISO-8601). May be `null` when no activity has been recorded. |
| `lastVisit` | `string \| null` | Last visit/login timestamp (ISO-8601). |

::: info No private fields
The `User` object never carries private data — no email address, private-message counts, subscription count, login/registration source, or email-campaign opt-in. These are stripped on the server for every method, including `Context.User()` for the authenticated viewer. Obtain private data server-side through a Connector if a widget genuinely needs it.
:::

***

## Guest Handling

When an unauthenticated visitor calls `ChWebSdk.Context.User()`, it resolves to the **guest projection** — a `User` object with `userId: null`, `username: "guest"`, and zeroed counts. It is never `null`, so always branch on `me.userId === null` before treating the viewer as a member.

For `ChWebSdk.User.list()` and related methods, guests get the public field set. Email-based `search` requires an authenticated session (guests receive a 403).

::: warning Private communities
On a **private community**, unauthenticated requests are redirected to the login page instead of returning data, so the SDK call rejects rather than resolving. Wrap calls in `try/catch` and treat a rejection as "not signed in."
:::

***

## Field Availability by Method

`User.search()` returns a reduced set optimized for autocomplete. Every other method — `Context.User()`, `getById()`, `getUsersById()`, `list()`, `getByRole()`, `getRecentlyActive()` — returns the full public `User` shape. No method exposes private data.

| Field group | `Context.User()` | Other `User.*` lookups | `search()` |
|---|---|---|---|
| Base scalars (`userId`, `username`, `avatar`, …) | ✅ | ✅ | Partial |
| `rank` | ✅ | ✅ | ✅ (without `id`) |
| `badges` | ✅ | ✅ | ✅ (without `id`) |
| `profileFields` | ✅ | ✅ | ❌ |
| Private data (email, PM counts, subscriptions, login/register source, email-campaign opt-in) | ❌ | ❌ | ❌ |

***

## Legacy Field Migration

If you are migrating code that reads `inSidedData.user`, use this table to find the canonical field name:

| Legacy field | Canonical field |
|---|---|
| `userid` (int) | `userId` |
| `name` | `username` |
| `url` | `profileUrl` |
| `userLevel` | `reputation` |
| `rankName`, `rankIcon` | `rank.name`, `rank.iconUrl` |
| `topicsCount` | `topics` |
| `repliesCount` | `replies` |
| `solvedCount` | `solved` |
| `likes` | `likesReceived` |
| `likes_given` | `likesGiven` |
| `joindate` (Unix int) | `joinDate` (ISO-8601 string — **different type**) |

Legacy `inSidedData.user` fields carrying private data — `email`, `pmUnreadCount`, `pmTotalCount`, `subscriptions`, `loginSource`, `registerSource` — have **no canonical equivalent**. They are not exposed by any SDK method; read them server-side through a Connector if required.

::: warning joinDate type change
`inSidedData.user.joindate` was a Unix timestamp (integer). The canonical `joinDate` is an ISO-8601 datetime **string**. If you compare or format dates, update your parsing logic.
:::

***

## Error Handling

All methods throw on failure. Wrap calls in `try/catch`:

```javascript
ChWebSdk.onReady(async () => {
  try {
    const me = await ChWebSdk.Context.User()
    if (me.userId === null) {
      // Guest — handle gracefully
      return
    }
    // Use me.username, me.rank, etc.
  } catch (err) {
    console.error('Failed to fetch user context:', err.message)
  }
})
```

Common error conditions:

| Scenario | Cause |
|---|---|
| `"Context.User can only be called in a browser environment"` | Called outside a browser (e.g. during SSR or in Node). |
| HTTP 400 | Invalid filter operator, an unsupported `sort` field (e.g. `sort=email`), or an unsupported filter passed to `list()`. |
| HTTP 403 | A guest called `User.search()` (authenticated-only), or passed an email-address term to `list({ search })`. Guest username search via `list()` is allowed. |
| HTTP 5xx | A server error occurred. Retry after a brief delay. |
| Rejected call (private community) | Unauthenticated requests are redirected to a login page instead of returning data; catch it and treat as "not signed in." |

The SDK surfaces these as a rejected promise (a thrown `Error`), **not** as a structured status code you can switch on — the "Scenario" column describes the underlying cause, not a machine-readable field. Branch on whether the call rejected, and inspect `err.message` for detail; there is no reliable `err.status` to implement per-code handling.

***

## Next Steps

* [Examples](examples) — runnable, copy-paste code for `Context.User()` and the `User.*` methods
* [Methods and Constructors](methods-constructors) — full reference for all SDK namespaces
* [Web SDK](overview) — availability and quick start
* [Passing User Context](/connectors/passing-user-context) — use server-side template variables when building Connectors that need the viewer's identity

QUICK REFERENCE — user context:

* Context.User() takes no arguments and always returns a User object, never null. A guest resolves to the guest projection (userId: null, username: "guest"). Branch on me.userId === null, not on me === null.
* No method exposes private data. email, pmUnread, pmTotal, subscriptions, loginSource, registerSource, and emailCampaignNotification are stripped server-side for every method, including Context.User() for the authenticated viewer. There is no getByEmail() method, no email filter parameter, and no email sort field. (A logged-in caller may still pass an email-shaped term to list({ search }); a guest doing so gets HTTP 403.)
* rank, badges, and profileFields are always hydrated on Context.User() and on every User lookup except search().
* User.getById(id) and User.getUsersById(ids) share one transport and return the identical full User shape. getById resolves null for a missing id; getUsersById silently omits unresolved ids and accepts up to 100 ids.
* User.list(options) returns totalItems plus an items array; defaults are page 1 and size 25, with a max size of 100. getByRole() and getRecentlyActive() are thin wrappers over list(). A guest passing an email-address term to list({ search }) gets HTTP 403; username search works for guests.
* User.search(query) returns up to 25 users (fixed, no size parameter) with a reduced field set for autocomplete UIs: no profileFields, and rank and badges carry no id. Use list() when you need the full object.
* userId is canonical; id, name, and userLevel are deprecated aliases. joinDate is an ISO-8601 string, not a Unix timestamp.
* All methods are browser-only and throw when no browser window is present.
