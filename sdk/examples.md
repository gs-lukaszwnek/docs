---
url: https://developer-portal.gainsight.com/docs/sdk/examples.md
description: >-
  Runnable code examples for the SDK — connector GET, POST, PUT, and DELETE
  requests, headers and timeouts, and Web SDK search, subscriptions, users, and
  script loading
---

# SDK Examples

Ready-to-use snippets for common SDK tasks. Each example is self-contained — copy it into your widget or sitewide script and adapt it.

::: info Prerequisites
These examples run in a widget, or in a sitewide script registered with `"type": "module"`, on a community page. Each imports `connectors` or `web` from `@gainsight-hub/sdk` — see [SDK](/sdk/) for setup and [SDK Reference](/sdk/reference) for every method and option.
:::

## Connectors

### GET request with query parameters

Fetch weather data by passing query parameters to the connector:

```javascript
import { connectors } from "@gainsight-hub/sdk";

const weather = await connectors.execute({
  permalink: "weather-api",
  method: "GET",
  queryParams: {
    q: "Warsaw",
    units: "metric"
  }
});

console.log(weather);
```

### POST request with JSON body

Create a resource by sending a JSON payload:

```javascript
import { connectors } from "@gainsight-hub/sdk";

const result = await connectors.execute({
  permalink: "crm-contacts",
  method: "POST",
  payload: {
    firstName: "Jane",
    lastName: "Doe",
    email: "jane@example.com"
  }
});

console.log(result);
```

### PUT request to update a resource

Update an existing resource by its identifier:

```javascript
import { connectors } from "@gainsight-hub/sdk";

const updated = await connectors.execute({
  permalink: "crm-contacts",
  method: "PUT",
  queryParams: { id: "contact-42" },
  payload: {
    firstName: "Jane",
    lastName: "Smith"
  }
});

console.log(updated);
```

### DELETE request

Remove a resource using DELETE:

```javascript
import { connectors } from "@gainsight-hub/sdk";

await connectors.execute({
  permalink: "task-manager",
  method: "DELETE",
  queryParams: { taskId: "task-99" }
});
```

### Custom headers per request

Pass additional headers for a single request without changing the client configuration:

```javascript
import { connectors } from "@gainsight-hub/sdk";

const data = await connectors.execute({
  permalink: "internal-api",
  method: "GET",
  headers: {
    "X-Request-Id": crypto.randomUUID(),
    "Accept-Language": "en-US"
  }
});
```

### Global headers with `configure`

Set headers that apply to every request made through `connectors`:

```javascript
import { connectors } from "@gainsight-hub/sdk";

connectors.configure({
  headers: {
    "X-App-Version": "2.1.0",
    "X-Client": "custom-widget"
  }
});

const data = await connectors.execute({
  permalink: "analytics-api",
  method: "GET"
});
```

### Custom timeout

Override the default 30-second timeout for long-running requests. `configure` applies to every widget on the page:

```javascript
import { connectors } from "@gainsight-hub/sdk";

connectors.configure({ timeout: 60000 });

const report = await connectors.execute({
  permalink: "report-generator",
  method: "POST",
  payload: { range: "last-90-days" }
});
```

### Handling errors with a styled fallback

Wrap `execute` calls in `try`/`catch` and render a small, styled fallback using design tokens so the widget degrades gracefully instead of showing an empty container. This example lives inside `init(sdk)`:

```javascript
import { connectors } from "@gainsight-hub/sdk"

export async function init(sdk) {
  await sdk.whenReady()

  const container = sdk.getContainer()
  container.innerHTML = `<div class="status">Loading…</div>`
  const status = sdk.$('.status')

  try {
    const data = await connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: { q: "Warsaw" }
    })

    status.textContent = `${data.city}: ${data.temperature}°C`
  } catch (error) {
    status.textContent = "Couldn't load weather data right now."
    status.style.color = "var(--color-content-subtle, #4F5663)"
    console.error("Connector request failed:", error)
  }
}
```

A plain-text fallback styled with the community's own design tokens reads as an intentional state, not a broken widget. See [Design Tokens Reference](/custom-widgets/v2/design-tokens-reference) for the full design-token catalog.

## Web SDK

### Search ideas and show titles

```javascript
import { web } from "@gainsight-hub/sdk";

const ideaInstance = web.Content.Idea()
const results = await ideaInstance.search('dashboard', { limit: 5 })
results.forEach((r) => console.log(r.title, r.url))
```

### Search with filters and pagination

```javascript
import { web } from "@gainsight-hub/sdk";

const results = await web.Content.search('bug', {
  contentType: ['idea', 'question'],
  categoryName: 'Ideas',
  limit: 20,
  page: 1,
  fetchMetadata: true
})
console.log(results)
```

### Like an idea (logged-in user)

```javascript
import { web } from "@gainsight-hub/sdk";

const idea = web.Content.Idea(98765)
await idea.like()
```

### Subscribe to a topic and check status

```javascript
import { web } from "@gainsight-hub/sdk";

await web.Subscription.subscribeToTopic('topic-abc')
const isSubscribed = await web.Subscription.getTopicSubscriptionStatus('topic-abc')
console.log(isSubscribed) // true
```

### Get the current user (viewer context)

```javascript
import { web } from "@gainsight-hub/sdk";

const me = await web.Context.User()

if (me.userId === null) {
  // Guest — show login prompt or public-only content
  console.log('Not logged in')
} else {
  // Authenticated viewer
  console.log(me.username)     // 'janedoe'
  console.log(me.userId)       // 101
  console.log(me.rank?.name)   // 'Regular'
  console.log(me.badges)       // [{ id: 100, title: 'First post', url: 'https://assets.example.com/…_thumb.png' }]
}
```

### Show the current user's badges

```javascript
import { web } from "@gainsight-hub/sdk";

const me = await web.Context.User()
if (me.userId !== null && me.badges.length > 0) {
  const list = document.createElement('ul')
  me.badges.forEach((badge) => {
    const li = document.createElement('li')
    li.textContent = badge.title
    list.appendChild(li)
  })
  document.getElementById('badges')?.appendChild(list)
}
```

### Personalize based on profile fields

```javascript
import { web } from "@gainsight-hub/sdk";

const me = await web.Context.User()
if (me.userId !== null) {
  // Find a profile field by ID (field IDs are specific to your community)
  const seniority = me.profileFields.find((f) => f.id === 3)
  if (seniority?.value === 'senior') {
    document.getElementById('advanced-content')?.removeAttribute('hidden')
  }
}
```

### Find users and use in UI

```javascript
import { web } from "@gainsight-hub/sdk";

const users = await web.User.getUsersById([1, 2, 3])
users.forEach((u) => console.log(u.userId, u.username, u.avatar))
```

### List moderators

```javascript
import { web } from "@gainsight-hub/sdk";

const { totalItems, items } = await web.User.list({
  role: [7], // role IDs are specific to your community
  sort: 'lastVisit',
  order: 'desc',
  size: 10
})
console.log(`${items.length} of ${totalItems} moderators`)
items.forEach((u) => console.log(u.username, u.rank?.name))
```

### Load a script

```javascript
import { web } from "@gainsight-hub/sdk";

web.loadScript("https://example.com/widget.js", () => {
  // Widget script is loaded
});
```

## Next Steps

* [SDK Reference](/sdk/reference) — full method signatures, options, and error handling
* [User Context](/sdk/user-context) — complete reference for `Context.User()`, all User methods, and the User object schema
* [Testing & Debugging](/connectors/testing-and-debugging) — verify your Connector setup works
* [Configuration](/connectors/configuration) — set up the connector your widget calls
* [Widgets repository template](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets) — working examples calling connectors
