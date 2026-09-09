---
url: https://developer-portal.gainsight.com/docs/connectors/configuration.md
description: >-
  Connector fields — endpoint URL, HTTP method, and routing settings, including
  which are required
---

# Configuration

Every connector requires a name, a destination URL, and an HTTP method. Everything else is optional and depends on what the external API expects.

The HTTP method you set here is fixed for this connector. Every SDK call must use the same method — the platform rejects requests that use a different method than the one configured.

## Required fields

* **Connection Name**: Unique, descriptive name tied to the system or use case. Use names like "Salesforce Case Creation" or "Weather API Current Conditions" — avoid generic labels like "API" or "Test". The platform derives a [permalink](#permalink) from this name.
* **Endpoint URL**: Full HTTPS endpoint. Supports template expressions in the path and query string — for example: https://api.example.com/users/{{ user.id }}/profile. The host accepts `get_variable()`, optionally with a `default()` fallback — see [Templating the host](#templating-the-host). The protocol cannot be templated. Non-templated URLs must be HTTPS and cannot target localhost or private IP ranges.
* **HTTP Method**: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, or `OPTIONS`.

## Optional fields

* **Headers**: Content type, API versions, or custom headers. See [Headers & Query Parameters](headers-and-query-parameters).
* **Query Parameters**: Search filters, pagination, or API keys. See [Headers & Query Parameters](headers-and-query-parameters).
* **Authentication Type**: API Key, OAuth Client Credentials, JWT, or OAuth JWT Bearer. Always use [Secrets and Variables](secrets/) for credential values. See [Authentication](authentication).
* **Request Transformation**: Template to reshape the request body for POST/PUT/PATCH requests. See [Request Transformation](payload-template).
* **Response Body**: Template to transform the upstream response before returning it to the widget. See [Response Transformation](response-transformation).
* **Response Content-Type**: Override the Content-Type header on the response returned to the widget.

## Templating the host

The host portion of the Endpoint URL accepts `get_variable()`, optionally wrapped in a `default()` fallback. No other template expression is valid there.

| Form | Example | Valid |
|------|---------|-------|
| Host from a Variable | https://{{ get\_variable('portal\_host') }}/api/v2 | Yes |
| Whole origin from a Variable | {{ get\_variable('instance\_url') }}/services/data | Yes |
| Host from a Variable, with a fallback | https://{{ get\_variable('portal\_host') | default('app.example.com', true) }}/api/v2 | Yes |
| One label of the host from a Variable | https://{{ get\_variable('region') }}.api.example.com/v1 | Yes |
| Any other expression in the host | https://{{ user.id }}.example.com/v1 | No |
| Templated protocol | {{ get\_variable('scheme') }}://api.example.com | No |

`get_variable()` must be called with a quoted name. Apart from `default()`, described below, nothing else may be applied to it: another filter ({{ get\_variable('host') | upper }}), a name held in another expression ({{ get\_variable(name) }}), or a conditional makes the URL invalid. The rejection message names the expression that caused it.

### Falling back to a default host

`default()` is the one filter allowed in the host. Use it when most communities should share a host and only some override it:

```jinja2
https://{{ get_variable('portal_host') | default('app.example.com', true) }}/api/v2
```

Its arguments must be written literally — a quoted string, optionally followed by `true`, and nothing else. An argument that reads another value, such as default(user.email), is rejected, as is any other number or type of argument.

When the Variable supplies the whole origin rather than just the host, the fallback has to carry the protocol too — default('https://app.example.com'), not default('app.example.com'). A fallback without it produces a URL with no protocol, which fails when the connector runs.

The second argument matters. With `true`, a Variable set to an empty value also falls back. Without it, only a Variable that has never been set falls back, and an empty one produces a URL with no host, which fails when the connector runs.

::: warning A default hides a misspelled Variable name
When a default is present, a Variable name that does not match anything no longer produces an error — the connector quietly uses the fallback host. That is what a default is for, but it means a typo can send live traffic to the wrong host silently. Use **Test Connection** to confirm the resolved URL is the one you expect.
:::

A templated host is checked when the connector runs, not when it is saved. The request fails if the host resolves to a non-HTTPS URL, to `localhost`, or to a private IP range — whether that value came from the Variable or from a `default()`. Without a default, a Variable that does not exist also fails, because the placeholder is left unresolved. Use **Test Connection** to see the resolved URL before relying on it.

Composite connector steps follow the same rules.

## Permalink

The **permalink** is a URL-safe identifier derived from the Connection Name (lowercase, spaces replaced by hyphens). You use it when calling the connector from widget code:

```javascript
sdk.connectors.execute({ permalink: "weather-api", method: "GET" });
```

::: info URL template limitations
URL templates render without access to secrets — use [Headers & Query Parameters](headers-and-query-parameters) or [Authentication](authentication) fields for credentials instead. A non-sensitive value in the URL, such as a per-environment host, comes from a **Variable** with `get_variable()`. See [Secrets and Variables](secrets/) for details.
:::

## Next Steps

* [Authentication](authentication) — Configure API keys or OAuth
* [Headers & Query Parameters](headers-and-query-parameters) — Add static or overridable values
* [Secrets and Variables](secrets/) — Store credentials securely
