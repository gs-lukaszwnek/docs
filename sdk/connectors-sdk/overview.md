---
url: https://developer-portal.gainsight.com/docs/sdk/connectors-sdk/overview.md
description: >-
  JavaScript client for calling connectors from widgets — reached as
  sdk.connectors on the sdk object passed to init(sdk)
---

# Connectors SDK

The Connectors SDK is a JavaScript client your widget code uses to call [connectors](/connectors/) — the secure backend proxies that connect widgets to external APIs. Calling an external API directly from the browser would expose your credentials, so the SDK exists to route each request through the platform instead, keeping those credentials on the backend.

It is available as `sdk.connectors` on the `sdk` object the platform passes to your widget's `init(sdk)` function. Nothing needs to be constructed or loaded — call a connector by its [permalink](/connectors/configuration):

```javascript
export async function init(sdk) {
  const data = await sdk.connectors.execute({
    permalink: "weather-api",
    method: "GET",
    queryParams: { q: "Warsaw" }
  });

  console.log(data);
}
```

For a task walkthrough, see [Calling from Widget Code](/connectors/calling-from-widgets).

See [SDK Concepts](/sdk/concepts) for how `sdk.connectors` relates to the rest of the `sdk` object, or the [Widget Runtime Reference](/sdk/runtime-reference) for the runtime API.

`sdk.connectors` is one client shared by every widget on the page. Per-request options such as `headers` affect only that call, but `sdk.connectors.configure(...)` changes the client for all widgets on the page — see `configure` in the [Reference](reference#configure).

::: warning Deprecated: `window.WidgetServiceSDK`
Constructing `new window.WidgetServiceSDK()` still works, but is no longer recommended. Use `sdk.connectors` from your widget's `init(sdk)` function instead.
:::

## Migrating from `window.WidgetServiceSDK`

| Before | After |
|--------|-------|
| `new window.WidgetServiceSDK()` then `sdk.connectors.execute(...)` | `sdk.connectors.execute(...)` |
| Constructor `headers` option | Per-request `headers` on `execute`, or `sdk.connectors.configure({ headers })` (applies to every widget on the page) |
| Constructor `csrfToken` option | `sdk.connectors.configure({ csrfToken })` (page-wide; normally read from the page automatically) |
| Constructor `timeout` option | `sdk.connectors.configure({ timeout })` or `sdk.connectors.setDefaultTimeout(ms)` (applies to every widget on the page) |

## Next Steps

* [Reference](reference) — full method signatures and error handling
* [Examples](examples) — real-world usage patterns
* [Configuration](/connectors/configuration) — set up connectors to call with the SDK
