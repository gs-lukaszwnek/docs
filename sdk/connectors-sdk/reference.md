---
url: https://developer-portal.gainsight.com/docs/sdk/connectors-sdk/reference.md
description: >-
  Complete API reference for the Connectors SDK (sdk.connectors) — access,
  configure(), execute() method, and error handling
---

# Reference

Complete reference for accessing `sdk.connectors`, its `configure` method, the `execute` method, and error behavior.

## Access

The Connectors SDK is the `connectors` property of the `sdk` object passed to your widget's `init(sdk)` function. Nothing needs to be constructed:

```javascript
export async function init(sdk) {
  const data = await sdk.connectors.execute({ permalink: "my-connector", method: "GET" });
}
```

`sdk.connectors` is one client shared by every widget on the page.

## `configure(options)` {#configure}

Change client defaults. Accepts a partial object of `csrfToken`, `headers`, and `timeout`:

```javascript
sdk.connectors.configure({
  timeout: 15000,
  headers: { "X-Custom-Header": "value" }
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `csrfToken` | `string` | Read from the page | CSRF token sent with requests; normally read from the page automatically |
| `headers` | `Record<string, string>` | `Content-Type` and `Accept` set to `application/json`; `configure` merges into them | Headers included with every request. Per-request `headers` on `execute` are merged on top for that call |
| `timeout` | `number` | `30000` | Request timeout in milliseconds |

::: warning Applies to every widget on the page
`sdk.connectors` is a single client shared by all widgets on the page, so `configure(...)` from one widget changes headers and timeout for all of them. Prefer per-request `headers` on `execute` when only one call needs them. `execute` takes no per-request timeout.
:::

Related methods on the same client: `updateHeaders(headers)`, `setDefaultTimeout(ms)`, `getHeaders()`, `getDefaultTimeout()`, `getBaseUrl()`.

## Constructor (deprecated) {#constructor}

::: warning Deprecated: `window.WidgetServiceSDK`
Constructing `new window.WidgetServiceSDK(config)` still works, but is no longer recommended. Use `sdk.connectors` from your widget's `init(sdk)` function instead.
:::

The old constructor took the same `headers` and `timeout` options as [`configure`](#configure). Instances built this way are separate from `sdk.connectors`:

```javascript
const sdk = new window.WidgetServiceSDK({
  timeout: 15000,
  headers: { "X-Custom-Header": "value" }
});
```

## `sdk.connectors.execute(options)`

Execute a connector and return the response.

```javascript
const result = await sdk.connectors.execute({
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
| `method` | `"GET" \| "POST" \| "PUT" \| "PATCH" \| "DELETE" \| "HEAD" \| "OPTIONS"` | Yes | — | HTTP method. Must match the method configured on the connector; a mismatch is rejected with HTTP 400 (METHOD\_NOT\_ALLOWED) |
| `payload` | `Record<string, JsonValue>` | No | `undefined` | The request body, as a plain JSON object. Pass it via `payload` (not `body`). Only sent for `POST`, `PUT`, `PATCH` methods |
| `queryParams` | `Record<string, string>` | No | `{}` | Query parameters appended to the request URL |
| `pathParams` | `Record<string, string \| number>` | No | `{}` | Values substituted into {{ pathParams.X }} URL template references on the connector. See [Dynamic URL Path Segments](/connectors/dynamic-url-paths) |
| `headers` | `Record<string, string>` | No | `{}` | Additional headers for this request |

`JsonValue` covers strings, numbers, booleans, null, arrays, and nested objects.

::: warning Pass the request body via `payload`, not `body`
The request body goes in the `payload` option as a plain JSON object. `body` is not a recognized option — and `execute()` silently ignores any option it does not recognize, so a call using `body` sends an empty request body.
:::

**Returns** `Promise` resolving to the parsed JSON response body (an object or an array). Rejects if the response body is not JSON, including an empty body.

### Path parameter validation

`pathParams` is validated by the SDK before any network call. The returned promise rejects, and no request is sent, on:

* An empty or whitespace-only key (e.g. `{ "": "x" }`)
* A value whose type is not `string | number` — catches untyped JS callers passing `null`, `undefined`, booleans, objects, or arrays
* A serialized header value over 4096 characters (gateway / CDN per-header limits typically sit around 8–16 KB; this cap leaves headroom)
* Passing `pathParams` to `sdk.connectors.composite.execute` (composite connectors don't accept path parameters)

Omitting `pathParams` or passing `{}` sends no header — the request goes out unchanged. See [Dynamic URL Path Segments — Validation and errors](/connectors/dynamic-url-paths#validation-and-errors) for the server-side error codes returned when validation passes the SDK but fails server-side.

## Error handling

`execute` returns a promise that rejects when the call fails — a network error, a timeout, an invalid `pathParams` value, or an error response from the connector or the API behind it. Wrap calls in `try`/`catch` and read `error.message`:

```javascript
try {
  const data = await sdk.connectors.execute({
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

## Next Steps

* [Examples](examples) — See real-world usage patterns with error handling
* [Calling from Widget Code](/connectors/calling-from-widgets) — How to call connectors using the SDK
* [Rendering & DOM](/custom-widgets/v2/rendering-and-dom) — Understand Shadow DOM context for DOM manipulation
