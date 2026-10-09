---
url: https://developer-portal.gainsight.com/docs/sdk/connectors-sdk/examples.md
description: >-
  Code examples for common sdk.connectors usage patterns — GET, POST, PUT, and
  DELETE requests, custom headers, and request timeouts
---

# Examples

Ready-to-use code examples for common widget patterns using the Connectors SDK. Each example shows a complete, self-contained call to `connectors.execute` that you can copy and adapt.

::: info Prerequisites
Each example runs inside your widget's `init(sdk)` function, where `sdk.connectors` is available without any setup. See [Connectors SDK](overview) for an introduction.
:::

## GET request with query parameters

Fetch weather data by passing query parameters to the connector:

```javascript
export async function init(sdk) {
  const weather = await sdk.connectors.execute({
    permalink: "weather-api",
    method: "GET",
    queryParams: {
      q: "Warsaw",
      units: "metric"
    }
  });

  console.log(weather);
}
```

## POST request with JSON body

Create a resource by sending a JSON payload:

```javascript
export async function init(sdk) {
  const result = await sdk.connectors.execute({
    permalink: "crm-contacts",
    method: "POST",
    payload: {
      firstName: "Jane",
      lastName: "Doe",
      email: "jane@example.com"
    }
  });

  console.log(result);
}
```

## PUT request to update a resource

Update an existing resource by its identifier:

```javascript
export async function init(sdk) {
  const updated = await sdk.connectors.execute({
    permalink: "crm-contacts",
    method: "PUT",
    queryParams: { id: "contact-42" },
    payload: {
      firstName: "Jane",
      lastName: "Smith"
    }
  });

  console.log(updated);
}
```

## DELETE request

Remove a resource using DELETE:

```javascript
export async function init(sdk) {
  await sdk.connectors.execute({
    permalink: "task-manager",
    method: "DELETE",
    queryParams: { taskId: "task-99" }
  });
}
```

## Custom headers per request

Pass additional headers for a single request without affecting the shared client configuration:

```javascript
export async function init(sdk) {
  const data = await sdk.connectors.execute({
    permalink: "internal-api",
    method: "GET",
    headers: {
      "X-Request-Id": crypto.randomUUID(),
      "Accept-Language": "en-US"
    }
  });
}
```

## Page-wide headers and timeout

Set headers or a timeout for every request made through `sdk.connectors`:

```javascript
export async function init(sdk) {
  sdk.connectors.configure({
    headers: { "X-App-Version": "2.1.0" },
    timeout: 60000
  });

  const report = await sdk.connectors.execute({
    permalink: "report-generator",
    method: "POST",
    payload: { range: "last-90-days" }
  });
}
```

::: warning Applies to every widget on the page
`sdk.connectors` is one client shared by all widgets on the page, so `configure(...)` changes headers and timeout for every widget, not just yours. Prefer per-request `headers` when only one call needs them. `execute` takes no per-request timeout.
:::

## Handling errors with a styled fallback

Wrap `execute` calls in `try`/`catch` and render a small, styled fallback using design tokens so the widget degrades gracefully instead of showing an empty container. This example also uses the widget runtime (`sdk.whenReady()`, `sdk.$()`) alongside `sdk.connectors`, and assumes your widget HTML contains `<div class="status">Loading…</div>`:

```javascript
export async function init(sdk) {
  await sdk.whenReady()

  const status = sdk.$('.status')
  status.textContent = "Loading…"

  try {
    const data = await sdk.connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: { q: "Warsaw" }
    })

    status.textContent = `${data.location.name}: ${data.current.temp_c}°C`
  } catch (error) {
    status.textContent = "Couldn't load weather data right now."
    status.style.color = "var(--color-content-subtle, #4F5663)"
    console.error("Connector request failed:", error)
  }
}
```

A plain-text fallback styled with the community's own design tokens reads as an intentional state, not a broken widget. See [Design Tokens Reference](/custom-widgets/v2/design-tokens-reference) for the full design-token catalog.

## Next Steps

* [Reference](reference) — Full SDK method signatures and error handling
* [Testing & Debugging](/connectors/testing-and-debugging) — Verify your Connector setup works
* [Configuration](/connectors/configuration) — Set up the connector your widget calls
* [Widgets repository template](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets) — Working examples calling connectors
