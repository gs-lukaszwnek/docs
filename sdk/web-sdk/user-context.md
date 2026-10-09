---
url: https://developer-portal.gainsight.com/docs/sdk/web-sdk/user-context.md
description: >-
  Complete reference for user context in the Web SDK — fetching the current
  viewer, looking up community members, the full User object schema, guest
  handling, and field availability by method.
---

# User Context

This page documents the Web SDK user-context API: fetching the current viewer with `sdk.web.Context.User()` and looking up community members with the `sdk.web.User.*` methods. All run as the visiting user, authorized by the community session cookie.

::: info Where the examples run
Examples run inside a widget's `init(sdk)` function, after checking that `sdk.web` is defined. It is `undefined` on embedded widgets and Skilljar host pages. [Script extensions](/custom-widgets/v2/scripts-overview) receive no `sdk` object; they use `window.ChWebSdk` instead. `window.ChWebSdk` in widget code still works but is deprecated — use `sdk.web`. Scripts keep using `window.ChWebSdk`. See [Web SDK](overview).
:::

::: tip
Prefer these SDK methods over a Connector for public member data — they use the browser session and need no server-side secrets. Private account data, such as email addresses, is not available through the SDK.
:::

## Context: Current Viewer

### `Context.User()`

Fetches the current user for the active session. This is the direct replacement for reading `inSidedData.user`.

**Returns:** `Promise<User>`

* Resolves to a `User` object for an authenticated visitor.
* For an unauthenticated (guest) visitor, resolves to the **guest projection** — a `User` object with `userId: null` and `username: "guest"`. It is **never `null`**; branch on `userId` to tell guest from member.
* Throws if called outside a browser environment or if the network request fails.

Takes no parameters. For a signed-in visitor, `badges` and `profileFields` are always present (an empty array when there is nothing to show) and `rank` is present when the user has one. The guest result has `badges: []` and no `profileFields` or `rank`; see [Guest Handling](#guest-handling).

::: warning Private profile fields are included
`Context.User()` returns the signed-in member's own profile fields, including the ones set to **Private** (`visibility: 2`). Private keeps a field from other members. It does not keep it from scripts running on the community page (analytics, tag managers, chat tools, other widgets), which can read it through the SDK. Do not send `profileFields` to external services or logs, and filter on `visibility === 1` when you need only public values. `User.getById()` called with the viewer's own ID returns public fields only.

Private account data is never included, even for the signed-in viewer: email address, private-message counts, subscription count, login/registration source, email-campaign opt-in, and custom role IDs.
:::

**Checking authenticated vs. guest:** branch on `userId`, not on `null`.

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return

  const me = await sdk.web.Context.User()

  if (me.userId === null) {
    // Guest — show a login prompt or public-only content
    return
  }

  // Authenticated — personalize the UI
  console.log(me.username)        // 'janedoe'
  console.log(me.userId)          // 101
  console.log(me.rank?.name)      // 'Regular'
  console.log(me.badges)          // [{ id: 100, title: 'First post', url: 'https://assets.example.com/…_thumb.png' }]
  console.log(me.profileFields)   // [{ id: 3, title: 'Seniority', value: 'senior', ... }]
}
```

***

## User: Lookup and Listing Methods

All methods below are **browser-only** and operate on the community user directory. They return the public `User` shape — no private account data (email, private-message counts, login source) and no private-visibility profile fields are exposed by any of them.

### `User.getById(id)`

Fetch a single user by numeric ID.

**Returns:** `Promise<User | null>` — resolves to `null` if the ID does not resolve.

| Parameter | Type | Description |
|---|---|---|
| `id` | `number \| string` | User ID. Numeric strings are accepted and coerced. |

```javascript
const user = await sdk.web.User.getById(101)
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
const users = await sdk.web.User.getUsersById([101, 202, 303])
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
const { items } = await sdk.web.User.list({ joinDate: 'P7D' })

// Users who joined in a specific range
const { items } = await sdk.web.User.list({
  joinDate: { from: '2024-01-01T00:00:00Z', to: '2024-12-31T23:59:59Z' }
})
```

**`NumberFilter`** — exact integer or `{ eq?, gt?, gte?, lt?, lte? }` range:

```javascript
// Users with 10 or more topics
const { items } = await sdk.web.User.list({ topics: { gte: 10 } })
```

**`UserSortField`** values: `'userId'` | `'username'` | `'replies'` | `'topics'` | `'points'` | `'lastVisit'` | `'joinDate'` | `'lastActivity'`

There is no sort by total post count. `topics` and `replies` sort separately, and any other `sort` value returns HTTP 400.

**Example — list moderators sorted by last visit:**

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return

  const { totalItems, items } = await sdk.web.User.list({
    role: [7],
    sort: 'lastVisit',
    order: 'desc',
    size: 20
  })
  console.log(`${items.length} of ${totalItems} moderators`)
  items.forEach((u) => console.log(u.username, u.lastVisit))
}
```

**Example — paginate through all users:**

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return

  const size = 100
  let page = 1
  let total = Infinity

  while ((page - 1) * size < total) {
    const result = await sdk.web.User.list({ page, size })
    total = result.totalItems
    result.items.forEach((u) => console.log(u.userId, u.username))
    page++
  }
}
```

### `User.getRecentlyActive(limit?)`

Convenience wrapper around `list()`. Returns users sorted by `lastVisit` descending.

**Returns:** `Promise<User[]>`

| Parameter | Type | Default | Description |
|---|---|---|---|
| `limit` | `number` | `10` | Maximum users to return. |

```javascript
const recentUsers = await sdk.web.User.getRecentlyActive(5)
recentUsers.forEach((u) => console.log(u.username, u.lastVisit))
```

### `User.getByRole(role, options?)`

List users whose main role is one of the specified roles. Custom roles are not matched.

**Returns:** `Promise<{ totalItems: number, items: User[] }>` — **defaults to 25 per page (max 100)**, same as `list()`.

| Parameter | Type | Description |
|---|---|---|
| `role` | `number \| number[]` | Role ID or array of IDs. OR semantics. |
| `options` | `object` | Same pagination and sort options as `list()` (minus `role`). |

```javascript
// Users with role id 7, first page
const { totalItems, items } = await sdk.web.User.getByRole(7, { size: 50 })

// Users with any of these roles
const { items } = await sdk.web.User.getByRole([7, 9])
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
| `profileUrl` | `string \| null` | Absolute URL to the profile page. |
| `avatar` | `string` | Avatar URL, or `""` when none. |
| `userTitle` | `string` | Display title. |
| `reputation` | `number \| null` | Reputation level, not the raw points. Omitted when no level is available for the user. |
| `isBanned` | `boolean` | `true` if the user holds the banned role. |
| `badges` | `Badge[]` | Hydrated badges, **without `id`**. |
| `rank` | `Rank \| null` | Hydrated rank, **without `id`**. |

```javascript
const results = await sdk.web.User.search('jane')
results.forEach((u) => console.log(u.userId, u.username, u.avatar))
```

***

## User Object Shape

Every method above except `search()` returns the unified `User` model. `search()` returns the reduced `UserSearchResult` shape described under `User.search(query)`.

A field with no value is **omitted** from the `User` object rather than returned as `null`. Fields marked *(optional)* in the tables below can be absent: read them with optional chaining (`user.rank?.name`) or a presence check, not `=== null`. `userId` and its mirror `id` are the exception: they are always present, and `null` on the guest result. Inside nested objects the rule differs: the `Rank` object returns its empty fields (such as `color` or `iconUrl`) as `null`, and a `ProfileField` `value` can be `null`.

**Not returned today:** `firstName`, `lastName`, `signature`, `customRoles`, `solved`, `likesReceived`, `likesGiven`, `followers` and `following`. The generated TypeScript `User` type still declares some of these as optional, but the backend does not populate them, so reading them always gives `undefined`.

### Core identity fields

| Field | Type | Notes |
|---|---|---|
| `userId` | `number \| null` | Canonical user ID. `null` only on the guest projection. Use this in new code. |
| `id` | `number \| null` | **Deprecated** mirror of `userId`. Kept for backward compatibility; migrate to `userId`. |
| `username` | `string` | Display username. `"guest"` on the guest projection. |
| `name` | `string` | **Deprecated** alias of `username`. |
| `avatar` | `string` | Avatar URL, or empty string `""` when none. |
| `profileUrl` | `string` (optional) | Absolute URL to the user's profile page (e.g. `"https://community.example.com/members/janedoe-101"`, where `101` is the user ID). The path holds the username in a URL-safe form (lowercase, symbols become hyphens: `mod_99` becomes `mod-99`), so use this value and do not build the link from `username`. Omitted on the guest result. |
| `url` | `string` (optional) | **Deprecated** mirror of `profileUrl`. Kept for backward compatibility; migrate to `profileUrl`. |
| `userTitle` | `string` (optional) | Display title (custom title or rank name; empty string if hidden). Omitted on the guest result. |
| `companyId` | `string` (optional) | The external ID of the company the user is linked to. Omitted when the user is not linked to a company. It is not the community's "Company" profile field, which is an ordinary text field in `profileFields`. |

### Role and moderation fields

| Field | Type | Notes |
|---|---|---|
| `isBanned` | `boolean` | `true` if the user holds the banned role. |
| `role` | `number` (optional) | Main role ID as an integer. Community-specific. Omitted on the guest result. |
| `isModerator` | `boolean` | Guest result only (`false`). A signed-in user's object never has it. |
| `mainRole` | `string` | Guest result only (`"roles.guest"`). A signed-in user's object never has it. |

A signed-in user's object has no moderator flag. `role` is the **main role** only: custom roles are not returned, and `getByRole()` and `list({ role })` match the main role only. To show something to moderators, compare `role` with your community's moderator, community manager and administrator role IDs (they are specific to each community, so look them up in the admin Roles settings and never assume the sample ID `7`). A role granted only as a custom role is not visible to the SDK.

### Rank fields

| Field | Type | Notes |
|---|---|---|
| `rankId` | `number` (optional) | Rank ID. Omitted when the user has no rank. |
| `rank` | `Rank` (optional) | Hydrated rank with display styling. Omitted when the user has no rank; see below. |

A user has no rank until the community's rank conditions assign one (a new member starts without one), and loses it when no rank condition matches. `rank` is also omitted when the community has turned the rank feature off or the rank no longer exists; in those two cases `rankId` can still be present, so test `rank`, not `rankId`, before rendering a rank.

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
| `url` | `string` | Absolute URL of the badge's thumbnail image (e.g. `"https://assets.example.com/attachment/3f2a_thumb.png"`). Empty string `""` when the badge has no image. |

### Group and profile fields

| Field | Type | Notes |
|---|---|---|
| `groups` | `number[]` | Group IDs the user belongs to. |
| `profileFields` | `ProfileField[]` | Custom profile fields stored for this user that the viewer is allowed to see, in the community's display order; see below. |

`profileFields` is not the community's full field list: one user can have 1 entry and another 17, so look a field up by `id`, never by position. To find a field's `id`, log your own `sdk.web.Context.User()` and read `profileFields`. A field titled "Email" is an ordinary profile field, not the account email address, which is never returned.

**`ProfileField` object shape:**

| Field | Type | Notes |
|---|---|---|
| `id` | `number` | Profile field ID |
| `title` | `string` | Profile field label (e.g. `"Seniority"`) |
| `type` | `string` | Field type: `'text'`, `'check'`, `'select'`, `'radio'`, `'birthday'` or `'date'`. |
| `value` | `string \| null` | The value as stored. An empty `text` field is `""`; an unset `select`, `check` or `date` field is `null`. Treat both as "no value". For `select` and `radio` fields it is the stored option value (for example `"india"`), not necessarily the label members see. |
| `visibility` | `number` | Visibility level of the field: `1` public, `2` private. Only `Context.User()` returns private fields (the viewer's own); every other method returns public ones. Hidden fields are never returned. |

### Activity and engagement fields

| Field | Type | Notes |
|---|---|---|
| `topics` | `number` | Topics created. |
| `replies` | `number` | Replies created. |
| `points` | `number` | Points. |
| `reputation` | `number` (optional) | Reputation level, not the raw points (`points` is a separate field). Omitted when no level is available for the user. |
| `userLevel` | `number` (optional) | **Deprecated** alias of `reputation`, returned by `getUsersById()` only. |

### Date fields

All three are UTC timestamps as ISO-8601 strings, written with a `+00:00` offset (for example `"2024-03-18T09:30:00+00:00"`). `new Date(value)` parses them.

| Field | Type | Notes |
|---|---|---|
| `joinDate` | `string` (optional) | Registration timestamp as ISO-8601 datetime (e.g. `"2024-03-18T09:30:00+00:00"`). Omitted on the guest result. **Not** a Unix timestamp — the legacy `inSidedData.user.joindate` was a Unix int; this is a string. |
| `lastActivity` | `string` (optional) | Last activity timestamp (ISO-8601). Equals `joinDate` until the user has activity. Omitted on the guest result. |
| `lastVisit` | `string` (optional) | Last visit/login timestamp (ISO-8601). Omitted when the user has never visited, and on the guest result. |

::: info No private account data
The `User` object never carries private account data — no email address, private-message counts, subscription count, login/registration source, email-campaign opt-in, or custom role IDs. These are stripped on the server for every method, including `Context.User()` for the authenticated viewer. Private profile fields are the one exception: only `Context.User()` returns them, and only the viewer's own. Obtain private account data server-side through a Connector if a widget genuinely needs it.
:::

***

## Guest Handling

When an unauthenticated visitor calls `sdk.web.Context.User()`, it resolves to the **guest projection** — a `User` object with `userId: null`, `username: "guest"`, and zeroed counts. It is never `null`, so always branch on `me.userId === null` before treating the viewer as a member.

The guest result contains only these keys: `userId` and `id` (both `null`), `username` and `name` (`"guest"`), `avatar` (`""`), `badges` and `groups` (both `[]`), `isBanned` (`false`), `isModerator` (`false`), `mainRole` (`"roles.guest"`), and `topics`, `replies` and `points` (all `0`). It has no `profileFields`, `rank`, `role`, `profileUrl` or date fields, so guard reads such as `me.profileFields` and `me.joinDate` behind the `userId === null` check.

For `sdk.web.User.list()` and related methods, guests get the public field set. Email-based `search` requires an authenticated session (guests receive a 403).

::: warning Private communities
On a **private community**, unauthenticated requests are redirected to the login page instead of returning data, so the SDK call rejects rather than resolving. Wrap calls in `try/catch` and treat a rejection as "not signed in."
:::

***

## Field Availability by Method

`User.search()` returns a reduced set optimized for autocomplete. Every other method — `Context.User()`, `getById()`, `getUsersById()`, `list()`, `getByRole()`, `getRecentlyActive()` — returns the full public `User` shape. No method exposes private account data, and only `Context.User()` returns private-visibility profile fields (the viewer's own).

| Field group | `Context.User()` | Other `User.*` lookups | `search()` |
|---|---|---|---|
| Base scalars (`userId`, `username`, `avatar`, …) | ✅ | ✅ | Partial |
| `rank` | ✅ | ✅ | ✅ (without `id`) |
| `badges` | ✅ | ✅ | ✅ (without `id`) |
| `profileFields` (public visibility) | ✅ | ✅ | ❌ |
| `profileFields` (private visibility, the viewer's own) | ✅ | ❌ | ❌ |
| Private account data (email, PM counts, subscriptions, login/register source, email-campaign opt-in, custom role IDs) | ❌ | ❌ | ❌ |

***

## Legacy Field Migration

If you are migrating code that reads `inSidedData.user` (or the `getUsersById` response, which carried `url` and `userLevel`), use this table to find the canonical field name:

| Legacy field | Canonical field |
|---|---|
| `userid` (int) | `userId` |
| `name` | `username` |
| `url` | `profileUrl` |
| `userLevel` | `reputation` |
| `rankName` | `rank.name` |
| `rankIcon` | `rank.avatarIconUrl` (`rank.iconUrl` is the separate username icon) |
| `topicsCount` | `topics` |
| `repliesCount` | `replies` |
| `joindate` (Unix int) | `joinDate` (ISO-8601 string — **different type**) |

Some legacy `inSidedData.user` fields have **no canonical equivalent**:

* Private data — `email`, `pmUnreadCount`, `pmTotalCount`, `subscriptions`, `loginSource`, `registerSource`. No SDK method exposes them; read them server-side through a Connector if required.
* Engagement counts — `solvedCount`, `likes`, `likes_given`. No SDK method returns them.
* Moderation — `isModerator`, `mainRole` and the comma-separated `role`. The SDK returns `isModerator` and `mainRole` only for guests, and `role` is the main role ID alone, so a script that gated on `isModerator` must compare `role` with your moderator role IDs instead. Custom roles are not returned.

::: warning Profile fields are broader than before
`inSidedData.user.profileFields` held public fields only. `Context.User().profileFields` also holds the viewer's own private fields, so a script that was safe to log or forward with the legacy list is not safe to do so with this one.
:::

::: warning joinDate type change
`inSidedData.user.joindate` was a Unix timestamp (integer). The canonical `joinDate` is an ISO-8601 datetime **string**. If you compare or format dates, update your parsing logic.
:::

***

## Error Handling

All methods throw on failure. Wrap calls in `try/catch`:

```javascript
export async function init(sdk) {
  await sdk.whenReady()
  if (!sdk.web) return

  try {
    const me = await sdk.web.Context.User()
    if (me.userId === null) {
      // Guest — handle gracefully
      return
    }
    // Use me.username, me.rank, etc.
  } catch (err) {
    console.error('Failed to fetch user context:', err.message)
  }
}
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
* [Reference](methods-constructors) — full reference for all SDK namespaces
* [Web SDK](overview) — availability and quick start
* [Passing User Context](/connectors/passing-user-context) — use server-side template variables when building Connectors that need the viewer's identity

QUICK REFERENCE — user context:

* Context.User() takes no arguments and always returns a User object, never null. A guest resolves to the guest projection (userId: null, username: "guest"). Branch on me.userId === null, not on me === null.
* No method exposes private account data, but Context.User() does include the viewer's own private profile fields (visibility 2); every other method returns only visibility 1. Never send profileFields to external URLs, analytics or logs, and filter on visibility === 1 when only public values are needed. email, pmUnread, pmTotal, subscriptions, loginSource, registerSource, emailCampaignNotification, and customRoles are stripped server-side for every method, including Context.User() for the authenticated viewer. There is no getByEmail() method, no email filter parameter, and no email sort field. (A logged-in caller may still pass an email-shaped term to list({ search }); a guest doing so gets HTTP 403.)
* For signed-in users badges and profileFields are always present on Context.User() and on every User lookup except search(). The guest result has badges: \[] and no profileFields, rank, role, profileUrl or date fields, so check me.userId === null before reading them. rank is present only when the user has one; test rank, not rankId.
* profileFields lists only the fields stored for that user and visible to the viewer, not every field the community defines, so look entries up by id. A value is exactly as stored: an empty text field is "", an unset select, check or date field is null.
* Timestamps (joinDate, lastActivity, lastVisit) are UTC ISO-8601 strings with a +00:00 offset, for example 2024-03-18T09:30:00+00:00. lastActivity equals joinDate until the user has activity; lastVisit is omitted for users who never visited. profileUrl contains a URL-safe form of the username plus the user id (mod\_99 becomes mod-99); never build it from username.
* User.getById(id) and User.getUsersById(ids) share one transport and return the identical full User shape. getById resolves null for a missing id; getUsersById silently omits unresolved ids and accepts up to 100 ids.
* User.list(options) returns totalItems plus an items array; defaults are page 1 and size 25, with a max size of 100. getByRole() and getRecentlyActive() are thin wrappers over list(). A guest passing an email-address term to list({ search }) gets HTTP 403; username search works for guests.
* User.search(query) returns up to 25 users (fixed, no size parameter) with a reduced field set for autocomplete UIs: no profileFields, and rank and badges carry no id. Use list() when you need the full object.
* userId is canonical; id, name, and url are deprecated aliases of userId, username, and profileUrl. getUsersById also returns a deprecated userLevel alias of reputation. joinDate is an ISO-8601 string, not a Unix timestamp. profileUrl is an absolute URL.
* A field with no value is omitted from the User object, not returned as null (userId and id are always present, null on the guest result). Fields marked optional in the tables can be absent; use optional chaining. Inside a rank object, empty fields (color, icon, iconUrl, avatarIcon, avatarIconUrl) are null, not omitted.
* The User object has no firstName, lastName, signature, customRoles, solved, likesReceived, likesGiven, followers, or following fields today. Do not read them; they are not returned. isModerator and mainRole exist only on the guest result (false and "roles.guest"); a signed-in user never has them, so they cannot detect moderators. role is the main role id only (custom roles are not returned); role, badge and profile-field ids are specific to each community, so ask for them and never assume a sample id such as 7.
* list() cannot sort by total post count. The sort fields are userId, username, replies, topics, points, lastVisit, joinDate, and lastActivity. Total post counts are not available, so never label any of those values as a post count.
* All methods are browser-only and throw when no browser window is present.
