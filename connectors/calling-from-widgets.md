---
url: https://developer-portal.gainsight.com/docs/connectors/calling-from-widgets.md
description: How to use the SDK to call connectors from widget code
---

# Calling from Widget Code

Call a connector by its [permalink](/connectors/configuration) using the [SDK](/sdk/). The SDK is loaded automatically by Customer Community and exposed on `window.WidgetServiceSDK` inside every widget — no script tag is required. The SDK sends the request to the backend connector instead of calling the destination API directly. See the [Widget SDK overview](/sdk/widget-sdk/overview) for constructor options.

## Call a connector

Use `sdk.connectors.execute()` with the connector's permalink. The `method` parameter must match the HTTP method configured on the connector (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, or `OPTIONS`).

A connector's permalink identifies a destination the community admin has already configured — including the target API and its credentials. Your widget code only references the permalink; it never chooses the destination or manages authentication for it.

The `(async () => { ... })();` wrapper is required because widget `<script>` tags are not ES modules — top-level `await` is only available in `<script type="module">`.

### GET with query parameters

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();
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
})();
```

### POST with JSON body

```javascript
(async () => {
  const sdk = new window.WidgetServiceSDK();
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
})();
```

::: warning Pass the request body via `payload`, not `body`
The request body goes in the `payload` option as a plain JSON object. `body` is not a recognized option — and `execute()` silently ignores any option it does not recognize, so a call using `body` sends an empty request body.
:::

See more usage patterns in [Examples](/sdk/widget-sdk/examples).

::: danger Never pass user identity from browser code
Data sent from the browser — including `queryParams` and `payload` — can be modified by the user. Never pass user IDs, emails, or other identity data as SDK parameters. Instead, use server-side template variables like {{ user.id }} in the connector configuration. See [Passing User Context](passing-user-context) for the secure pattern.
:::

## Common mistakes

These patterns compile but fail at runtime or in production. The examples above use a bare `sdk` for the connector-calling instance — that's fine in a standalone script like these, since nothing else named `sdk` is in scope. The mistake below only arises once a widget's `init(sdk)` function is *also* in scope in the same code. See [SDK Concepts](/sdk/concepts) for why these are separate objects in the first place. The two distinctly-named objects — `widgetSdk` for the `init(sdk)` parameter, `widgetServiceSdk` for the instance constructed here — get confused with each other in that case:

| Wrong | Why it fails |
|-------|-------------|
| `widgetSdk.connectors.execute(...)` (called from inside `init(widgetSdk)`) | The [Widget Runtime Reference](/custom-widgets/v2/sdk-api-reference) passed to `init` has no `.connectors` property — it is a different object from the connector-calling instance on this page. Throws `Cannot read properties of undefined (reading 'execute')`. Construct your own instance under a distinct name, e.g. `const widgetServiceSdk = new window.WidgetServiceSDK();` — see the [full example](/sdk/widget-sdk/examples#handling-errors-with-a-styled-fallback) that does this inside `init(sdk)`. |
| `widgetServiceSdk.execute(...)` | `.execute` lives under `.connectors`, not on the top-level instance. Throws `TypeError: widgetServiceSdk.execute is not a function`. |
| `const { connectors } = widgetServiceSdk; connectors.execute(...)` | Works, but destructuring obscures which SDK instance you're calling through. Write both dot segments explicitly: `widgetServiceSdk.connectors.execute(...)`. |
| `fetch("https://api.example.com/...")` | A raw `fetch()` to an external domain is blocked in production. The platform sets no explicit `connect-src` in its Content-Security-Policy, so `connect-src` falls back to `default-src 'self'` — same-origin requests only. Route every external call through `widgetServiceSdk.connectors.execute(...)` instead, which proxies the request server-side. |

## Working with the response

Widget code runs inside Shadow DOM. If you update the DOM after a connector call, query elements through the widget's shadow root, not `document`. See [Rendering & DOM](../custom-widgets/v2/rendering-and-dom).

## Next Steps

* [Examples](/sdk/widget-sdk/examples) — More usage patterns and advanced options
* [Passing User Context](passing-user-context) — Securely inject user identity into requests
* [Testing & Debugging](testing-and-debugging) — Verify your connector works and diagnose issues
* [Template Variables](template-variables) — Variables and functions available in templates
