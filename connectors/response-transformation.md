---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/response-transformation.md
description: >-
  Response transformation reference — the Response Body and Response
  Content-Type fields, the response variables available to templates, and
  response template error handling
---

# Response Transformation

A [Jinja2](https://jinja.palletsprojects.com/) template applied to the upstream API response body before it is returned to the widget — letting you filter, reshape, or reformat responses on the server.

The template applies to the **incoming response body only** — it has no effect on the outgoing request. If the field is blank, the original upstream response is returned as-is.

## Fields

### Response Body

A [Jinja2](https://jinja.palletsprojects.com/) template applied to the upstream API response. The template has access to the full response body, status code, and headers.

If blank or not set, the upstream response is passed through unchanged.

### Response Content-Type

Overrides the `Content-Type` header on the response returned to the widget. If blank, the upstream API's original `Content-Type` is preserved.

Set this when your template changes the output format — for example, if the upstream API returns XML but your template produces JSON, set this to `application/json`.

## Variables

::: tip
For the full list of variables, filters, and functions available in templates, see [Template Variables](template-variables).
:::

Three variables are specific to response templates:

* **`response.body`** — the upstream response body as a decoded UTF-8 string. If the response is not valid UTF-8 (e.g. binary content), this is `None`.
* **`response.status_code`** — the HTTP status code from the upstream API (integer).
* **`response.headers`** — the response headers from the upstream API as a dictionary. Header names are lowercase (e.g., `content-type`, not `Content-Type`).

Also available: `user.*` fields, `tenant_id`, `now()`, and filters such as `from_json`, `json_encode`, `tojson`, `base64_encode`, and `base64_decode` — standard Jinja2 built-in filters (e.g. `selectattr`, `list`) are available too.

::: warning get\_secret() is not available here
Response templates do not have access to {{ get\_secret() }}. Response output is returned to the browser, so secrets are excluded to prevent accidental leakage. Use headers or query parameters if you need to include secret values in the request to the upstream API. See [Secrets and Variables](./secrets/) for details.
:::

::: warning jwt\_encode() is not available here
Response templates do not have access to `jwt_encode()`. JWTs are used for authenticating outgoing requests, not for shaping response data.
:::

::: warning body\_text and body\_raw are not available
{{ body\_text }} and {{ body\_raw }} are not available in response templates. These variables represent the incoming request body — use `response.body` to access the upstream API's response.
:::

## Examples

**Extract specific fields from a large response:**

```jinja2
{% set data = response.body | from_json %}
{
  "id": {{ data.id }},
  "name": "{{ data.name }}",
  "status": "{{ data.status }}"
}
```

For more worked examples — handling errors by status code, including user context, and other common transformations — see [Response Transformation Recipes](response-transformation-recipes).

::: danger Do not output from\_json directly
Never use {{ response.body | from\_json }} as the entire response template — it produces HTML-escaped Python notation, not valid JSON. Leave the field blank to forward the response as-is, or use {% set %} to extract specific fields. See [Template Variables — Behavioral notes](template-variables#behavioral-notes) for details.
:::

## Error handling

If the response template fails to render (e.g. syntax error, type mismatch), the connector returns HTTP 422 with a JSON error envelope whose `errors.scope` is "Configuration (response template rendering)". This is separate from request template errors, which use "Configuration (template rendering)".

The response template runs on all upstream status codes — including 4xx and 5xx errors. This means you can normalize error responses from different APIs into a consistent format for your widget.

## Next Steps

* [Response Transformation Recipes](response-transformation-recipes) — Worked examples and common gotchas for reshaping, filtering, and resolving response data
* [Calling from Widget Code](calling-from-widgets) — Use connectors with response transformation in your widget
* [Filtering Sensitive Data](filtering-sensitive-data) — Use response templates to keep hidden records out of the browser
* [Template Variables](template-variables) — Full reference for Jinja2 variables, filters, and functions
* [Jinja2 Template Designer Docs](https://jinja.palletsprojects.com/en/stable/templates/) — official reference for Jinja2 syntax: loops, conditionals, filters, and `namespace`
* [Configuration](configuration) — Overview of all connector fields
* [Request Transformation](payload-template) — Reshape outgoing request bodies with Jinja2
