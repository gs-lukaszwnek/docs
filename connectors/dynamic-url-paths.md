---
url: https://developer-portal.gainsight.com/docs/connectors/dynamic-url-paths.md
description: >-
  Use caller-supplied values as path segments in connector URLs, with URL-safe
  validation
---

# Dynamic URL Path Segments

Most REST APIs put resource identifiers in the URL path (`/users/{id}/posts`, `/ideas/{id}/endorsements`). To call these from a widget, declare each variable segment as a **path parameter** and reference it in the connector URL with {{ pathParams.X }}. Callers supply the value at run time; the platform validates it as URL-safe and substitutes it into the URL.

This avoids the alternative of either creating one connector per resource ID, or hacking the value into the upstream API's query string when the upstream expects it in the path.

## Quick example

A connector fetching a single idea's endorsements declares `idea_id` as a path parameter, references it in the connector URL as {{ pathParams.idea\_id }}, and the widget supplies the value per call:

```javascript
// Connector URL: https://api.example.com/ideas/{{ pathParams.idea_id }}/endorsements
const data = await sdk.connectors.execute({
  permalink: "idea-endorsements",
  method: "GET",
  pathParams: { idea_id: 42 }
});
// Outbound: GET https://api.example.com/ideas/42/endorsements
```

## Connector definition

Declare each path parameter as a row in the connector's `path_parameters` array. The row shape mirrors [`headers`](headers-and-query-parameters) and [`query_parameters`](headers-and-query-parameters): `{key, value?, overridable?}`. The `key` names the parameter — it must match a {{ pathParams.X }} reference in `url`, and it is the same name the SDK caller uses on `pathParams`.

```json
{
  "url": "https://api.example.com/ideas/{{ pathParams.idea_id }}/endorsements",
  "path_parameters": [
    { "key": "idea_id" }
  ]
}
```

Complete connector definitions live in [Repository Registry](repository-registry) (for repo-synced connectors) and [Code Mode](code-mode) (for admin-UI JSON).

## Validation rule

Every path-parameter value is validated against one universal rule:

* Allowed characters: **letters, digits, and `. _ ~ -`** (the [RFC 3986 unreserved set](https://datatracker.ietf.org/doc/html/rfc3986#section-2.3))
* Length: 1–200 characters
* Rejected: empty values, whitespace, `/`, `?`, `#`, `%`, spaces, control characters, and path-traversal segments (`.`, `..`, anything containing `..`)

Accepts: `42`, `user-42`, `USER_42`, `my.handle`, `a~b`, RFC 4122 UUIDs in any case.
Rejects: empty, `has space`, `with/slash`, `with?q`, `..`, `%20`, `with#hash`.

Path parameters and [query parameters](headers-and-query-parameters) are configured in separate sections of the connector — path-parameter values are substituted into {{ pathParams.X }} template references in the URL path, while query-parameter entries are appended to the query string.

## Authorization

The platform validates that path-parameter values are URL-safe, but it does not authorize the caller's access to the resource they identify. A widget supplying `pathParams: { user_id: 42 }` has `42` substituted into the URL whether or not the authenticated user is user 42 — the same way a query parameter or overridable header would.

Two patterns keep this safe:

* **Upstream enforces it.** The destination API rejects requests the caller is not entitled to. Typical for production REST APIs with per-resource ACLs.
* **Pin the value server-side.** When the path parameter must equal the authenticated user (or another value the browser cannot change), drop the path parameter and put a server-side template variable like {{ user.id }} directly in the connector URL. See [Passing User Context](passing-user-context).

## Validation and errors

Values are validated server-side **before** any outbound call is made — the upstream API never sees a malformed input. The connector execution endpoint returns a 400 with a structured error body on validation failure:

| `code` | When it fires | `errors` shape |
|---|---|---|
| `INVALID_PATH_PARAMETERS` | At least one caller-supplied value fails URL-safe validation (wrong characters, too long, path-traversal segment, etc.) | `{ code, params: string[] }` — flat list of failing keys |
| `MISSING_PATH_PARAMETERS` | The URL template references {{ pathParams.X }} but no value was supplied for `X` (one or more keys) | `{ code, params: string[] }` — flat list of missing keys |
| `CONNECTOR_MISCONFIGURED` | The URL references a path parameter that has no declared row in `path_parameters` | `{ code, detail }` |

Missing wins over invalid: if any required key is missing, only `MISSING_PATH_PARAMETERS` is returned; value validation runs only when all required keys are supplied. The substituted value is percent-encoded before reaching the upstream URL.

## Composite connectors

Path parameters are **not supported** on [composite connectors](composite-connectors). The SDK's `sdk.connectors.composite.execute` throws `ConnectorBoundaryError` synchronously if `pathParams` is supplied — no request is sent. If you need per-call path values in a composite flow, compose simple connectors from inside the composite's step URLs using the step result variables.

## SDK reference

The `pathParams` field on `sdk.connectors.execute` accepts `Record<string, string | number>`. The SDK serializes the values into an `X-Path-Params` HTTP header in urlencoded form (same wire shape as a query string) and the server reads them from there. You don't need to handle the encoding — the SDK does it.

```javascript
await sdk.connectors.execute({
  permalink: "user-posts",
  method: "GET",
  pathParams: { user_id: 42, post_slug: "hello-world" }
});

// Outbound:
//   GET /connectors/user-posts/execute
//   X-Path-Params: user_id=42&post_slug=hello-world
```

Boundary validation runs synchronously before any network call:

| Trigger | SDK behaviour |
|---|---|
| Empty / whitespace-only key (`{ "": "x" }`) | Throws `ConnectorBoundaryError` |
| Value whose type is not `string` or `number` (e.g. `null`, an object, a boolean) | Throws `ConnectorBoundaryError` |
| Serialized payload over 4096 characters | Throws `ConnectorBoundaryError` |
| Passed to `composite.execute` with at least one key | Throws `ConnectorBoundaryError` |
| Omitted or `{}` | No header sent — request goes out unchanged |

`ConnectorBoundaryError` is distinct from `ConnectorExecuteError` (which normalises server-side failures from the response body). For path-parameter validation failures the server returns the envelope shape `{ success: false, errors: { code, params, detail }, request_id }`; `ConnectorExecuteError` exposes those as optional `code`, `params`, and `detail` fields. Branch on `err.code` to handle each error:

```javascript
import { ConnectorBoundaryError, ConnectorExecuteError } from "@gainsight-hub/connectors-sdk";

try {
  await sdk.connectors.execute({ permalink: "...", method: "GET", pathParams: { id } });
} catch (err) {
  if (err instanceof ConnectorBoundaryError) {
    // Client-side rejection — fix the caller
  } else if (err instanceof ConnectorExecuteError) {
    switch (err.code) {
      case "MISSING_PATH_PARAMETERS":
      case "INVALID_PATH_PARAMETERS":
        // err.params: string[] — the failing keys
        break;
      case "CONNECTOR_MISCONFIGURED":
        // err.detail: string — operator-facing detail
        break;
      default:
        // Legacy shape (or an unrecognised envelope code) — err.failureScope / err.reason / err.requestId / err.status
    }
  }
}
```

## Next steps

* [Headers & Query Parameters](headers-and-query-parameters) — for values that go in the query string, not the URL path
* [Template Variables](template-variables) — full reference for Jinja2 expressions in connector URLs and templates
* [Calling from Widget Code](calling-from-widgets) — full SDK usage for widgets
* [Testing & Debugging](testing-and-debugging) — verify path parameters substitute correctly

1. Connector definition side (connectors\_registry.json / Code Mode JSON): declare each parameter as a row in `path_parameters`. Row shape mirrors `headers` and `query_parameters`: `{ "key": "<param_name>" }`. Example: `"path_parameters": [{ "key": "idea_id" }]`.

2. SDK caller side (sdk.connectors.execute): pass `pathParams` as an object whose *object keys* are the parameter names. Example: `pathParams: { idea_id: 42 }`.

The connector-side value (`"idea_id"`) and the SDK-side key (`idea_id`) must match, and both must match the `{{ pathParams.X }}` reference in the connector `url`.
