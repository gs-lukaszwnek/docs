---
url: https://developer-portal.gainsight.com/docs/connectors/secrets.md
description: >-
  Named key-value pairs — Secrets (masked credentials) and Variables (plain-text
  config) — referenced from connector configuration via
  get_secret()/get_variable()
---

# Secrets and Variables

**Secrets** and **Variables** are named key-value pairs you store once and reference from any connector. A **Secret** holds a sensitive value — an API key, token, or password — and is masked in the UI. A **Variable** holds a non-sensitive, plain-text value — a base URL, environment ID, or instance ID — and stays visible so you can see exactly what a connector uses.

Secrets and Variables are managed in **Integrations** → **Developer Studio** → **Secrets and Variables**, where you can create, edit, and delete both.

## Choosing between a Secret and a Variable

Choose the type when you add a value. The choice controls how the value is shown and where it can be used.

| | Secret | Variable |
|--|--------|----------|
| Use it for | Sensitive credentials — API keys, tokens, passwords | Non-sensitive config — base URLs, environment IDs, instance IDs |
| Shown in the UI | Masked | Visible in full |
| Reference with | `get_secret('name')` | `get_variable('name')` |
| Allowed in the connector **URL** | No | **Yes** |
| Available in headers, query parameters, and authentication | Yes | Yes |

::: warning Variables are visible — never store credentials in them
A Variable's value is plain text and shown in full in the UI. Use Variables only for non-sensitive configuration. For any credential — an API key, token, or password — always use a Secret.
:::

Both are scoped to your community and resolved on the server when a connector executes, so a Variable used in a URL is applied server-side and cannot be tampered with by the browser.

## Secrets

Secrets store sensitive values that must never be exposed. Their values are encrypted, masked in the UI — only the first character is shown — and never returned to the browser.

### Referencing secrets in connectors

Use the `get_secret()` function in a template expression to reference a stored secret:

```jinja2
{{ get_secret('secret_name') }}
```

The expression is replaced with the actual secret value when the connector executes. The value is resolved on the server and never exposed to the browser.

#### Where get\_secret() is available

| Field | Available | Why |
|-------|-----------|-----|
| Authentication config | **Yes** | Auth values are used only in outgoing requests |
| Headers | **Yes** | Request headers are server-side only |
| Query Parameters | **Yes** | Request params are server-side only |
| URL template | **No** | Secrets stripped to prevent URLs with embedded tokens |
| Request payload | **No** | Payload forwarded to upstream API |
| Response transform | **No** | Response returned to browser, secrets excluded for safety |

**Key principle:** Secrets resolve only in fields that control the **outgoing request to the external API** — authentication, headers, and query parameters. They do not resolve in fields that may leak data (URL, request bodies, response bodies) to prevent accidental exposure of credentials.

**Security note on URLs:** Never embed {{ get\_secret('...') }} in the URL template. Secrets are removed before the URL is built, so the reference is left unresolved and the connector fails to execute — and any token placed in a URL would be logged and could appear in browser history. Use the authentication config, headers, or query parameters instead. If you need a **non-sensitive** value in the URL — such as a per-environment host — use a [Variable](#variables).

To send a Bearer token, use the **Authentication** config or a custom **Header** — not the URL:

```json
{
  "type": "api_key",
  "config": {
    "key": "Authorization",
    "value": "Bearer {{ get_secret('my_token') }}",
    "in": "header"
  }
}
```

This keeps the token out of URLs, logs, and browser history.

## Variables

Variables store non-sensitive, reusable configuration values — base URLs, environment identifiers, instance IDs. Their values stay visible in the UI, so you can confirm exactly what a connector will call.

Variables make connectors **environment-aware**. Store a value once under a descriptive name — such as `salesforce_instance_url` or `api_environment` — then reference that name from a connector instead of hardcoding the value. A community used for development can point at a sandbox host while production points at a live host, by changing one Variable rather than the connector.

### Referencing variables in connectors

Use the `get_variable()` function in a template expression:

```jinja2
{{ get_variable('variable_name') }}
```

Unlike secrets, variables also resolve inside the **URL** field, so you can parameterize a connector's host or path per environment. Store the full base URL as the variable's value — for example, set `salesforce_instance_url` to `https://acme.my.salesforce.com` — then build the rest of the path around it:

```jinja2
{{ get_variable('salesforce_instance_url') }}/services/data/v59.0/query
```

The value is resolved on the server when the connector executes.

#### Where get\_variable() is available

In a standard connector, `get_variable()` resolves everywhere `get_secret()` does — authentication, headers, and query parameters — **and additionally in the URL template**. It does not resolve in a standard connector's request payload or response transformation templates.

| Field | Available | Why |
|-------|-----------|-----|
| Authentication config | **Yes** | Resolved server-side in outgoing requests |
| Headers | **Yes** | Request headers are server-side only |
| Query Parameters | **Yes** | Request params are server-side only |
| URL template | **Yes** | Variables are non-sensitive, so they are kept when the URL is built |
| Request payload | **No** | Not available in a standard connector's request template |
| Response transform | **No** | Not available in a standard connector's response template |

In [composite connector](../composite-connectors) steps, `get_variable()` is available in **every** step template — including the request body and response body — because all step templates share one context. See [Composite Connector Reference](../composite-connector-reference#available-in-step-templates).

## Naming rules

Secret and variable names must be valid identifiers — letters, numbers, and underscores only. Names are case-sensitive.

**Valid names:**

* `salesforce_instance_url`
* `api_environment`
* `oauth_client_id`
* `weather_api_key`

**Invalid names (rejected on save):**

* `instance-url` (contains hyphen)
* `api key` (contains space)
* `123env` (starts with number)
* `api@key` (contains special character)

Use descriptive names that include the service and purpose: `salesforce_instance_url`, `helpdesk_subdomain`, `stripe_webhook_secret`.

A name cannot be used for both a Secret and a Variable in the same community — each name is unique across both.

For diagnosing values that fail to resolve, validation errors when saving, or other issues, see [Testing & Debugging](../testing-and-debugging#secrets-and-variables-issues).

## Next Steps

* [Authentication](../authentication) — Use secrets in API Key, OAuth, and JWT authentication
* [Template Variables](../template-variables) — Full reference of variables and functions available in templates
* [Testing & Debugging](../testing-and-debugging) — Verify secrets and variables resolve correctly
