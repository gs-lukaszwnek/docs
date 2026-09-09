---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/composite-connector-reference.md
description: Composite connector step fields, template variables, and error types
---

# Composite Connector Reference

Field, variable, and error reference for [Build a Composite Connector](composite-connectors).

## Step fields

Each step in a composite connector's `steps` array supports the following fields.

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `name` | string | Yes | — | Unique step identifier used as steps.\<name> in templates. Slug format: lowercase letters, digits, and hyphens (e.g. `auth`, `fetch-user`). Use bracket notation for hyphenated names: steps\['fetch-user'].body. |
| `url` | string | Yes | — | HTTPS URL for the API call. Supports Jinja2 templates. |
| `method` | string | No | `GET` | HTTP method. One of `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`. |
| `headers` | array | No | `[]` | Request headers as `{key, value, overridable}` objects. `overridable` defaults to `false` when omitted. |
| `query_parameters` | array | No | `[]` | URL query parameters as `{key, value, overridable}` objects. |
| `authentication` | object | No | none | Authentication strategy as `{type, config}`. Same options as regular connectors — see [Authentication](authentication). |
| `request_body` | string | No | `""` | Jinja2 template for the request body. |
| `response_body` | string | No | `""` | Jinja2 template applied to the step's response before the result is stored in steps.\<name>.body. |
| `response_content_type` | string | No | `""` | `Content-Type` override. Applies only to the final step's response returned to the widget. |

## Step result variables

After a step completes, its response is available to subsequent steps and to the final response:

| Variable | Type | Description |
|----------|------|-------------|
| steps.\<name>.body | string or `None` | Response body as a UTF-8 string. `None` when the response is binary. If the step has a `response_body` template, this holds the transformed output instead of the raw body. |
| steps.\<name>.status\_code | integer | HTTP status code returned by the step. |
| steps.\<name>.headers | dict | Response headers (lowercase keys). |

## Request data

The client's request data is exposed to step templates via the `request` object:

| Variable | Type | Description |
|----------|------|-------------|
| `request.body_text` | string or `None` | Request body as text. `None` when the body is binary or empty. |
| `request.body_raw` | binary or `None` | Request body as raw binary. Use `body_text` for text payloads. |
| `request.headers` | dict | Only `Content-Type` is available. Other client headers are not forwarded to step templates. |
| `request.query_parameters` | dict | Query parameters from the client's request URL. |

## Available in step templates

Each step template has access to the following variables and functions:

| Variable or function | URL and request body templates | Header, query param, and authentication templates | Response body templates |
|---------------------|-------------------------------|--------------------------------------------------|-----------------|
| steps.\<name>.\* | Yes | Yes | Yes |
| `request.*` | Yes | Yes | Yes |
| `user.*` | Yes | Yes | Yes |
| `tenant_id` | Yes | Yes | Yes |
| `get_secret()` | No | Yes | No |
| `get_variable()` | Yes | Yes | Yes |
| `jwt_encode()` | Yes | Yes | No |
| `now()` | Yes | Yes | Yes |

For the full list of Jinja2 filters and functions, see [Template Variables](template-variables).

## SDK

`sdk.connectors.composite.execute(options)` executes a composite connector from widget code and returns a Promise that resolves with the parsed response body.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `options.permalink` | string | Yes | The composite connector's permalink or numeric ID. |
| `options.payload` | object | No | Request body forwarded as the POST payload. Pass a plain object — the SDK handles serialisation. Do not pass a `JSON.stringify`'d string. |
| `options.queryParams` | object | No | Query parameters appended to the request URL. Available in step templates as `request.query_parameters.<key>`. |

The HTTP method is always POST. It is not configurable by the caller — the SDK owns this detail.

```html
<script>
(async () => {
  const sdk = new window.WidgetServiceSDK();
  try {
    const data = await sdk.connectors.composite.execute({
      permalink: "auth-and-fetch-user",
      payload: { userId: "usr_123" },
      queryParams: { format: "json" }
    });
    console.log("Result:", data);
  } catch (error) {
    console.error("Connector request failed:", error);
  }
})();
</script>
```

Errors (step 4xx/5xx, template errors, network errors) reject the returned Promise with an error object consistent with errors thrown by `sdk.connectors.execute()`.

## Response headers

| Header | Description |
|--------|-------------|
| `Server-Timing` | Per-step duration entries (`step.<name>;dur=<ms>`) plus `total`. |
| `X-Connector-Timing` | JSON object with per-step timing breakdown. |

## Error responses

All step failures return a JSON error envelope whose `errors.scope` identifies the step:

| Scenario | HTTP status | errors.scope |
|----------|-------------|-------------|
| Step returns HTTP 4xx/5xx | 502 | Composite step '\<name>' (index \<N>): Downstream (HTTP error) |
| Step network error | 502 | Composite step '\<name>' (index \<N>): Downstream (network/external service) |
| Step template rendering error | 422 | Composite step '\<name>' (index \<N>): Configuration (template rendering) |
| Step JWT signing error | 422 | Composite step '\<name>' (index \<N>): Authentication (JWT signing) |
| Composite has no steps | 500 | Unexpected error |

No data from previous successful steps is included in error responses.

## Limitations

* Steps execute sequentially. Parallel execution is not supported.
* Composite connectors are managed exclusively through the repository registry. No admin UI or API endpoints for CRUD.
* The response returned to the widget is always the last step's response. There is no composite-level response template.
* Each step is a standalone definition. Steps cannot reference existing regular connectors by permalink.

## Next Steps

* [Build a Composite Connector](composite-connectors) — Define, execute, and debug composite connectors
* [Authentication](authentication) — Authentication strategies for individual steps
* [Template Variables](template-variables) — Full reference for Jinja2 variables, filters, and functions
