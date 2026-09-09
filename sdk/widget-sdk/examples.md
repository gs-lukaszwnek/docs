---
url: https://developer-portal.gainsight.com/docs/sdk/widget-sdk/examples.md
description: >-
  Code examples for common WidgetServiceSDK usage patterns — GET, POST, PUT, and
  DELETE requests, custom headers, and request timeouts
---

# Examples

Ready-to-use code examples for common widget patterns using the SDK. Each example shows a complete, self-contained call to `connectors.execute` that you can copy and adapt.

::: info Prerequisites
These examples run inside a widget hosted in Customer Community, where `window.WidgetServiceSDK` is globally available. See [Widget SDK](overview) for constructor options.
:::

## GET request with query parameters

Fetch weather data by passing query parameters to the connector:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();

  const weather = await sdk.connectors.execute({
    permalink: "weather-api",
    method: "GET",
    queryParams: {
      q: "Warsaw",
      units: "metric"
    }
  });

  console.log(weather);
})();
```

## POST request with JSON body

Create a resource by sending a JSON payload:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();

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
})();
```

## PUT request to update a resource

Update an existing resource by its identifier:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();

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
})();
```

## DELETE request

Remove a resource using DELETE:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();

  await sdk.connectors.execute({
    permalink: "task-manager",
    method: "DELETE",
    queryParams: { taskId: "task-99" }
  });
})();
```

## Custom headers per request

Pass additional headers for a single request without affecting the global SDK configuration:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();

  const data = await sdk.connectors.execute({
    permalink: "internal-api",
    method: "GET",
    headers: {
      "X-Request-Id": crypto.randomUUID(),
      "Accept-Language": "en-US"
    }
  });
})();
```

## Global headers via constructor

Set headers that apply to every request made through the SDK instance:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK({
    headers: {
      "X-App-Version": "2.1.0",
      "X-Client": "custom-widget"
    }
  });

  const data = await sdk.connectors.execute({
    permalink: "analytics-api",
    method: "GET"
  });
})();
```

## Custom timeout

Override the default 30-second timeout for long-running requests:

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK({
    timeout: 60000
  });

  const report = await sdk.connectors.execute({
    permalink: "report-generator",
    method: "POST",
    payload: { range: "last-90-days" }
  });
})();
```

## Handling errors with a styled fallback

Wrap `execute` calls in `try`/`catch` and render a small, styled fallback using design tokens so the widget degrades gracefully instead of showing an empty container. This example lives inside `init(sdk)` — the only example on this page that does — so the connector-calling instance is named `widgetServiceSdk` rather than `sdk`, avoiding a collision with the `sdk` parameter already in scope (see [Common mistakes](/connectors/calling-from-widgets#common-mistakes) for what goes wrong if you don't):

```javascript
export async function init(sdk) {
  await sdk.whenReady()

  const container = sdk.getContainer()
  container.innerHTML = `<div class="status">Loading…</div>`
  const status = sdk.$('.status')

  try {
    const widgetServiceSdk = new window.WidgetServiceSDK()
    const data = await widgetServiceSdk.connectors.execute({
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

## Next Steps

* [Widget SDK Methods and Constructors](methods-constructors) — Full SDK method signatures and error handling
* [Testing & Debugging](/connectors/testing-and-debugging) — Verify your Connector setup works
* [Configuration](/connectors/configuration) — Set up the connector your widget calls
* [Widgets repository template](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets) — Working examples calling connectors
