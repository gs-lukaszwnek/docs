---
url: https://developer-portal.gainsight.com/docs/connectors/template-variables.md
description: >-
  Reference of Jinja2 variables and functions available in connector request and
  response templates
---

# Template Variables

Connector fields — URLs, headers, query parameters, authentication values, payload bodies, and response bodies — support [Jinja2](https://jinja.palletsprojects.com/) templating. This lets you inject dynamic values into requests and responses. Not all functions are available in every field — see the availability table below for details.

## Available variables

Only the variables below are available in connector templates. Custom data and arbitrary request context are not available.

**User fields**

```jinja2
{{ user.email }}
{{ user.first_name }}
{{ user.id }}
{{ user.last_name }}
{{ user.username }}
```

**Profile fields**

Access custom profile fields by their numeric ID (found in your community's Profile Fields settings):

```jinja2
{{ user.profile_fields.by_id(123) }}
```

**Other variables**

```jinja2
{{ tenant_id }}   {# your community's unique identifier #}
{{ body_text }}   {# request payload as a decoded UTF-8 string — payload templates only #}
{{ body_raw }}    {# request payload as raw bytes — payload templates only; renders as b'...' in a template, prefer body_text for JSON #}
```

Composite connector steps reference this same data as `request.body_text` and `request.body_raw` instead — see [Composite Connector Reference](composite-connector-reference#request-data).

**Response variables** (response templates only)

```jinja2
{{ response.body }}          {# upstream response body as a UTF-8 string; None for non-UTF-8/binary responses #}
{{ response.status_code }}   {# HTTP status code from the upstream API #}
{{ response.headers }}       {# response headers from the upstream API (dict, lowercase keys) #}
```

## Filters

| Filter | Description |
|--------|-------------|
| `from_json` | Parse a JSON string into an object: `body_text \| from_json` |
| `json_encode` | Serialize a value to a JSON string |
| `tojson` | Serialize a value to JSON (built-in Jinja2 filter) |
| `base64_encode` | Base64-encode a string |
| `base64_decode` | Decode a base64 string |

## Functions

| Function | Description |
|----------|-------------|
| `get_secret('name')` | Retrieve a stored secret by name. See [Secrets and Variables](secrets/). |
| `get_variable('name')` | Retrieve a stored variable — a non-sensitive, plain-text config value — by name. Available in the URL as well as headers, query parameters, and auth. It is also the only expression accepted in the URL host — see [Templating the host](configuration#templating-the-host). See [Secrets and Variables](secrets/). |
| `jwt_encode(payload, key, algorithm, headers)` | Sign a JWT. `payload`: dict of claims, `key`: signing secret (str), `algorithm`: algorithm string (e.g. `'HS256'`, `'RS256'`), `headers`: optional dict of additional JWT header fields (e.g. `{'kid': 'my-key-id'}`) |
| `now(offset=0)` | Return the current Unix timestamp as an integer. `offset` is an optional number of seconds to add (positive for future, negative for past). Example: `now(3600)` returns a timestamp 1 hour from now. |

::: info Stored Variables vs. template variables
`get_secret()` and `get_variable()` look up values you store in the [Secrets and Variables](secrets/) section. Those stored **Variables** are a different concept from the template variables documented on this page (like `user.*` and `tenant_id`), which come from the request context.
:::

## Where each variable is available

This table covers regular connector templates. For composite connector step templates, see [Available in step templates](composite-connector-reference#available-in-step-templates) — the variable set and per-context rules differ (composite steps use `request.*` instead of bare `body_text`/`body_raw`, and add `steps.<name>.*`).

| Variable | URL | Headers | Query Params | Auth | Request Transform | Response Transform |
|----------|:---:|:-------:|:------------:|:----:|:----------------:|:-----------------:|
| `user.*` |  |  |  |  |  |  |
| `tenant_id` |  |  |  |  |  |  |
| `get_secret()` |  |  |  |  |  |  |
| `get_variable()` |  |  |  |  |  |  |
| `jwt_encode()` |  |  |  |  |  |  |
| `now()` |  |  |  |  |  |  |
| `body_text` |  |  |  |  |  |  |
| `body_raw` |  |  |  |  |  |  |
| `response.body` |  |  |  |  |  |  |
| `response.status_code` |  |  |  |  |  |  |
| `response.headers` |  |  |  |  |  |  |

## Examples

```jinja2
{# Pass user email to the external API #}
{{ user.email }}

{# Read a custom profile field #}
{{ user.profile_fields.by_id(42) }}

{# Parse and extract a field from the incoming request body #}
{{ (body_text | from_json).order.id }}

{# Reference a stored secret #}
{{ get_secret('api_key') }}

{# Sign a JWT with an expiry 1 hour from now #}
{{ jwt_encode({'sub': user.email, 'exp': now(3600)}, get_secret('jwt_secret'), 'HS256') }}

{# Sign a JWT with a key ID header (useful when the receiver validates against multiple keys) #}
{{ jwt_encode({'sub': user.email, 'exp': now(3600)}, get_secret('jwt_secret'), 'HS256', {'kid': 'my-key-id'}) }}

{# Extract a field from the upstream response (response templates only) #}
{% set data = response.body | from_json %}
{{ data.results[0].name }}

{# Branch on upstream status code (response templates only) #}
{% if response.status_code >= 400 %}
{ "error": true, "upstream_status": {{ response.status_code }} }
{% endif %}
```

## Behavioral notes

* **Anonymous users**: All `user.*` properties return `None` for unauthenticated users. Rendering `None` produces the literal string `"None"` — guard with {{ user.email | default('') }}.
* **Authentication enforcement**: If headers or query parameters reference {{ user. variables, anonymous requests are rejected with an error.
* **Jinja2 control flow**: Standard Jinja2 features like `{% set %}`, `{% if %}`, `{% for %}` are available for building complex templates.
* **`from_json` errors**: If the input is not valid JSON, the connector execution fails.
* **`from_json` output**: Never use {{ body\_text | from\_json }} or {{ response.body | from\_json }} as a standalone expression. It renders as HTML-escaped Python dict notation, not JSON. Use `from_json` with `{% set %}` to extract specific fields, or leave the template blank to forward the body as-is. See [Request Transformation](payload-template) and [Response Transformation](response-transformation) for details.

## Next Steps

* [Request Transformation](payload-template) — Use Jinja2 to reshape request bodies
* [Response Transformation](response-transformation) — Transform upstream API responses with Jinja2
* [Secrets and Variables](secrets/) — Store credentials and config values referenced with `get_secret()` and `get_variable()`
* [Authentication](authentication) — Apply Jinja2 expressions in auth fields
* [Build a Composite Connector](composite-connectors) — Multi-step templating with `steps.*` and `request.*`
