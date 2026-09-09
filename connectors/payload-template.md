---
url: https://developer-portal.gainsight.com/docs/connectors/payload-template.md
description: >-
  Request transformation — the Jinja2 template for the outgoing request body,
  and the body_text and body_raw variables available to it
---

# Request Transformation

A [Jinja2](https://jinja.palletsprojects.com/) template that defines the body of the outgoing HTTP request — letting you reshape data before it reaches the destination and control exactly what is sent.

The template applies to the **outgoing request body only** — it has no effect on the response. If the field is blank, the original incoming payload is forwarded as-is.

::: warning
When the request transformation field is blank, the complete original request body is forwarded to the external API. This may include sensitive user data or internal fields. Set a request transformation to control exactly what is sent.
:::

Use request transformation for POST, PUT, and PATCH requests where you need to control the request body structure. GET requests have no body, so this field has no effect on GET connectors.

## Variables

::: tip
For the full list of variables, filters, and functions available in templates, see [Template Variables](template-variables).
:::

Two variables are specific to payload templates:

* **`body_text`** — the incoming request body decoded as a UTF-8 string. Use this for JSON payloads.
* **`body_raw`** — the request body as raw bytes. Use only when you need exact binary encoding. Avoid rendering it directly in a Jinja2 template — the output is not valid JSON.

Both are available for POST, PUT, and PATCH requests. For GET requests (or any request with no body), both are `None`. If you render a `None` variable in a template, it produces the literal string `None`, not an empty string — so always guard with a default filter (e.g., {{ body\_text | default('') }}) when the variable may be absent.

::: info Where `body_text` comes from
`body_text` is this template's view of the incoming request body. The widget sends that body through the SDK's `payload` option — see [Widget SDK Methods and Constructors](/sdk/widget-sdk/methods-constructors) — and it arrives here as `body_text`.

This is a separate layer from the declarative `body` field used in [Dynamic Options](/custom-widgets/v2/widget-schema#api-endpoint-object) and the [Widget Definition Reference](/custom-widgets/v2/widget-schema#content-object).
:::

::: warning get\_secret() is not available here
Unlike headers and query parameters, payload templates do not have access to `get_secret()`. Secrets are intentionally excluded from payload templates. Use headers or query parameters if you need to include a secret value. See [Secrets and Variables](./secrets/) for details.
:::

## Examples

```jinja2
{
  "tenant_id": "{{ tenant_id }}",
  "user_email": "{{ user.email }}",
  "original_payload": "{{ body_text }}"
}
```

```jinja2
{
  "tenant_id": "{{ tenant_id }}",
  "order_id": "{{ (body_text | from_json).order.id }}",
  "order_total": {{ (body_text | from_json).order.total }}
}
```

::: tip
Numeric fields like `order_total` above are not wrapped in quotes so they render as JSON numbers, not strings. If the downstream API expects a number type, omitting the quotes ensures the value is sent correctly.
:::

::: danger Do not output from\_json directly
Never use {{ body\_text | from\_json }} as the entire payload template — it produces HTML-escaped Python notation, not valid JSON. Leave the template blank to forward the body as-is, or use {% set %} to extract specific fields. See [Template Variables — Behavioral notes](template-variables#behavioral-notes) for details.
:::

## Next Steps

* [Calling from Widget Code](calling-from-widgets) — Use payload templates in your widget connector calls
* [Template Variables](template-variables) — Full reference for Jinja2 variables, filters, and functions
* [Configuration](configuration) — Overview of all connector fields
* [Response Transformation](response-transformation) — Transform upstream API responses with Jinja2
