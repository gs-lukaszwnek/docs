---
url: https://developer-portal.gainsight.com/docs/connectors/calling-from-widgets.md
description: How to call connectors from widget code with sdk.connectors
---

# Calling from Widget Code

Call a connector by its [permalink](/connectors/configuration) using the [SDK](/sdk/). Call connectors through `sdk.connectors` on the `sdk` object your widget's `init(sdk)` function receives — no script tag, import, or construction is required. The SDK sends the request to the backend connector instead of calling the destination API directly. See the [Connectors SDK overview](/sdk/connectors-sdk/overview) for the full API.

## Call a connector

Use `sdk.connectors.execute()` with the connector's permalink. The `method` parameter must match the HTTP method configured on the connector (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, or `OPTIONS`).

A connector's permalink identifies a destination the community admin has already configured — including the target API and its credentials. Your widget code only references the permalink; it never chooses the destination or manages authentication for it.

The examples below show code inside your widget's `init(sdk)` function.

### GET with query parameters

```javascript
export async function init(sdk) {
  try {
    const data = await sdk.connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: {
        q: "Warsaw"
      }
    });
    console.log(data);
  } catch (error) {
    console.error("Connector request failed:", error);
  }
}
```

### POST with JSON body

```javascript
export async function init(sdk) {
  try {
    const result = await sdk.connectors.execute({
      permalink: "plantbook",
      method: "POST",
      payload: { name: "Monstera", size: "M" }
    });
    console.log(result);
  } catch (error) {
    console.error("Connector request failed:", error);
  }
}
```

::: warning Pass the request body via `payload`, not `body`
The request body goes in the `payload` option as a plain JSON object. `body` is not a recognized option — and `execute()` silently ignores any option it does not recognize, so a call using `body` sends an empty request body.
:::

See more usage patterns in [Examples](/sdk/connectors-sdk/examples).

::: danger Never pass user identity from browser code
Data sent from the browser — including `queryParams` and `payload` — can be modified by the user. Never pass user IDs, emails, or other identity data as SDK parameters. Instead, use server-side template variables like {{ user.id }} in the connector configuration. See [Passing User Context](passing-user-context) for the secure pattern.
:::

## Common mistakes

These patterns compile but fail at runtime or in production:

| Wrong | Why it fails |
|-------|-------------|
| `sdk.execute(...)` | `.execute` lives under `.connectors`, not on the `sdk` object. Throws `TypeError: sdk.execute is not a function`. Write `sdk.connectors.execute(...)`. |
| `fetch("https://api.example.com/...")` | A raw `fetch()` to an external domain is blocked in production. The platform sets no explicit `connect-src` in its Content-Security-Policy, so `connect-src` falls back to `default-src 'self'` — same-origin requests only. Route every external call through `sdk.connectors.execute(...)` instead, which proxies the request server-side. |

## Working with the response

Widget code runs inside Shadow DOM. If you update the DOM after a connector call, query elements through the widget's shadow root, not `document`. See [Rendering & DOM](../custom-widgets/v2/rendering-and-dom).

## Next Steps

* [Examples](/sdk/connectors-sdk/examples) — More usage patterns and advanced options
* [Passing User Context](passing-user-context) — Securely inject user identity into requests
* [Testing & Debugging](testing-and-debugging) — Verify your connector works and diagnose issues
* [Template Variables](template-variables) — Variables and functions available in templates
