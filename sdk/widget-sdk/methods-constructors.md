---
url: >-
  https://developer-portal.gainsight.com/docs/sdk/widget-sdk/methods-constructors.md
description: >-
  Complete API reference for the WidgetServiceSDK JavaScript class —
  constructor, execute() method, and error handling
---

#### Widget SDK

# Methods and Constructors

Complete reference for the `WidgetServiceSDK` constructor, the `connectors.execute` method, and error behavior.

## Constructor

```javascript
const sdk = new window.WidgetServiceSDK(config);
```

The `config` argument is optional. Pass a configuration object when creating the SDK instance:

```javascript
const sdk = new window.WidgetServiceSDK({
  timeout: 15000,
  headers: { "X-Custom-Header": "value" }
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `headers` | `Record<string, string>` | `{}` | Headers included with every request |
| `timeout` | `number` | `30000` | Request timeout in milliseconds |

## `connectors.execute(options)`

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

### Path parameter validation

`pathParams` is validated synchronously at the SDK boundary before any network call. The SDK throws `ConnectorBoundaryError` on:

* An empty or whitespace-only key (e.g. `{ "": "x" }`)
* A value whose type is not `string | number` — catches untyped JS callers passing `null`, `undefined`, booleans, objects, or arrays
* A serialized header value over 4096 characters (gateway / CDN per-header limits typically sit around 8–16 KB; this cap leaves headroom)
* Passing `pathParams` to `sdk.connectors.composite.execute` (composite connectors don't accept path parameters)

Omitting `pathParams` or passing `{}` sends no header — the request goes out unchanged. See [Dynamic URL Path Segments — Validation and errors](/connectors/dynamic-url-paths#validation-and-errors) for the server-side error codes returned when validation passes the SDK but fails server-side.

## Error handling

Wrap `execute` calls in a try/catch to handle network errors, timeouts, and connector failures:

```javascript
import {
  ConnectorBoundaryError,
  ConnectorExecuteError
} from "@gainsight-hub/connectors-sdk";

try {
  const data = await sdk.connectors.execute({
    permalink: "my-connector",
    method: "GET",
    pathParams: { user_id: 42 }
  });
  console.log(data);
} catch (error) {
  if (error instanceof ConnectorBoundaryError) {
    // Client-side rejection — invalid pathParams shape, composite + pathParams, etc.
    console.error("Boundary rejection:", error.reason);
  } else if (error instanceof ConnectorExecuteError) {
    switch (error.code) {
      case "MISSING_PATH_PARAMETERS":
      case "INVALID_PATH_PARAMETERS":
        // error.params: string[] — the failing keys
        console.error("Path params failed:", error.code, error.params);
        break;
      case "CONNECTOR_MISCONFIGURED":
        // error.detail: string — operator-facing detail
        console.error("Misconfigured:", error.detail);
        break;
      default:
        // Legacy shape — error.failureScope / error.reason / error.requestId / error.status
        console.error("Execute failed:", error.failureScope, error.reason, error.status);
    }
  } else {
    // Network / timeout / unknown
    console.error("Connector execution failed:", error.message);
  }
}
```

`ConnectorBoundaryError` is thrown synchronously before any HTTP call — it signals a malformed call. `ConnectorExecuteError` normalises two server-error shapes: the envelope `{ success: false, errors: { code, detail, scope?, params? }, request_id }` returned for connector execution errors — path-parameter validation (HTTP 400) as well as connection, template, and authentication failures (HTTP 422/502/500) — exposed as optional `code` / `params` / `detail` fields, and the legacy `{ failure_scope, reason, request_id }` (exposed as `failureScope` / `reason`). Branch on `error.code` to handle envelope errors; fall back to `failureScope` for legacy ones.

Common failure scenarios:

| Scenario | Cause | Error class |
|----------|-------|-------------|
| Network error | The backend is unreachable | `Error` |
| Timeout | Request exceeded the configured `timeout` (default 30s) | `Error` |
| Connector not found | The `permalink` does not match any existing connector | `Error` (HTTP 404) |
| Authentication failure | The connector's auth credentials are invalid or expired | `ConnectorExecuteError` |
| Upstream error | The destination API returned an error | `ConnectorExecuteError` |
| Invalid `pathParams` shape | Empty key, wrong value type, oversized payload, or passed to composite | `ConnectorBoundaryError` |
| Missing required path param value | URL references {{ pathParams.X }} but caller omitted one or more keys from `pathParams` | `ConnectorExecuteError` (`code: "MISSING_PATH_PARAMETERS"`) |
| Path param value fails URL-safe validation | Caller supplied a value containing characters outside `[A-Za-z0-9._~-]`, or a path-traversal segment (`..`) | `ConnectorExecuteError` (`code: "INVALID_PATH_PARAMETERS"`) |

## Next Steps

* [Examples](examples) — See real-world usage patterns with error handling
* [Calling from Widget Code](/connectors/calling-from-widgets) — How to call connectors using the SDK
* [Rendering & DOM](/custom-widgets/v2/rendering-and-dom) — Understand Shadow DOM context for DOM manipulation
