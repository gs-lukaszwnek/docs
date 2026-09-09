---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/filtering-sensitive-data.md
description: >-
  Prevent sensitive API data from reaching the browser by filtering responses in
  the connector definition instead of in widget code
---

# Filtering Sensitive Data

Use this guide when the external API returns more data than the browser should see — hidden records returned alongside public ones, fields that identify other users, or any record the logged-in user is not entitled to read.

## Prerequisites

* An existing connector configured in **Integrations** → **Developer Studio** → **Connectors** (see [Build Your First Connector](build-first-connector))
* Knowledge of what the external API returns — inspect a live response with [Testing & Debugging](testing-and-debugging) before you start filtering

## The response is always visible to the browser

Every connector response travels back to the widget that called it. Widget code runs in the browser, so everything the external API returned — hidden records, internal IDs, private emails, machine-only fields — is visible in the browser's Network tab.

A filter written in widget JavaScript only controls **what the widget renders**. It does not hide the raw data. A user can open dev tools and read the full response regardless of what the UI displays.

::: danger Wrong: filtering in widget code

```javascript
// The full response is already in the browser
const sdk = new window.WidgetServiceSDK();
const data = await sdk.connectors.execute({
  permalink: "groups",
  method: "GET"
});
const publicGroups = data.groups.filter(g => !g.hidden);
// data still contains every hidden group in the Network tab
```

:::

## Filter before the response leaves the server

Two server-side mechanisms keep sensitive data out of the browser. Choose the one that matches the external API.

### Option 1 — Filter in the request

If the external API accepts a filter parameter (for example, `visibility=public` or `type=user`), add it as a non-overridable query parameter. The server never fetches the sensitive records in the first place.

In the connector's **Query Parameters**:

* **Query Key**: `visibility` — **Query Value**: `public` — **Overridable**: unchecked

Non-overridable means widget code cannot change the value at run time. See [Headers & Query Parameters](headers-and-query-parameters#overridable-fields).

### Option 2 — Filter in the response template

If the external API returns everything and offers no filter parameter, add a [Response Transformation](response-transformation) that strips sensitive records before the response is returned.

```jinja2
{% set data = response.body | from_json %}
{
  "groups": [
    {% for group in data.groups if group.is_public %}
      { "id": {{ group.id }}, "name": {{ group.name | tojson }} }
      {% if not loop.last %},{% endif %}
    {% endfor %}
  ]
}
```

Replace `data.groups` and `group.is_public` with the field names used by your external API — inspect a live response in [Testing & Debugging](testing-and-debugging) to confirm.

## Example: Community Groups

The Community API endpoint `GET /v2/category/getTree?module[]=groups` returns every group when called with [OAuth Client Credentials](authentication#oauth-client-credentials), including groups that are hidden from most end users. The raw response reaches the browser unchanged unless the connector filters it.

::: danger Wrong: no filter, hidden groups leak

```json
{
  "name": "Groups",
  "url": "https://api2-us-west-2.insided.com/v2/category/getTree",
  "method": "GET",
  "headers": [
    { "key": "Accept", "value": "application/json", "overridable": true }
  ],
  "query_parameters": [
    { "key": "module[]", "value": "groups", "overridable": true }
  ],
  "authentication": {
    "type": "oauth_client_credentials",
    "config": {
      "scope": "read write",
      "client_id": "{{ get_secret('client_id') }}",
      "client_secret": "{{ get_secret('client_secret') }}",
      "token_url": "https://api2-us-west-2.insided.com/oauth2/token"
    }
  }
}
```

Every group in the community — public, hidden, private — appears in the browser's Network tab. Filtering the array in widget JavaScript hides the hidden groups from the UI but not from a curious user.
:::

::: tip Right: response template strips hidden groups
Add a [Response Transformation](response-transformation) that keeps only the groups the browser should see. In the **Response Body** field:

```jinja2
{% if response.status_code == 200 %}
{% set tree = response.body | from_json %}
{
  "groups": [
    {% for group in tree.categories if group.is_public %}
      {
        "id": {{ group.id }},
        "name": {{ group.name | tojson }},
        "slug": {{ group.slug | tojson }}
      }
      {% if not loop.last %},{% endif %}
    {% endfor %}
  ]
}
{% else %}
{ "error": true, "status": {{ response.status_code }} }
{% endif %}
```

Set **Response Content-Type** to `application/json`. Replace `tree.categories` and `group.is_public` with the actual field names your API returns — verify against a live response.

The `status_code` guard matters because response templates run on **all** upstream status codes, including 4xx and 5xx — see [Response Transformation — Error handling](response-transformation#error-handling). Without the guard, a non-JSON error body would crash `from_json` and return a generic template-rendering failure.

The hidden groups are dropped on the server. Neither the widget nor the browser ever receives them.
:::

## Machine-to-machine authentication needs extra filtering

[OAuth Client Credentials](authentication#oauth-client-credentials), [JWT](authentication#jwt), and [OAuth JWT Bearer](authentication#oauth-jwt-bearer) authenticate the connector as a service account — not as the logged-in user. The external API applies no per-user permission checks, so the response reflects everything the service account is allowed to read.

If the logged-in user must only see a subset of that data, you are responsible for narrowing the response in the connector definition. Without filtering, the browser receives every record the service account can read — including records the user was never meant to access.

## Checklist before publishing a connector

* \[ ] Inspect the full upstream response in [Testing & Debugging](testing-and-debugging) — is there anything the browser should not see?
* \[ ] If yes, add a filter query parameter (Overridable unchecked) or a response template that strips sensitive fields
* \[ ] Never rely on widget JavaScript to hide sensitive data — the raw response is always visible in the Network tab
* \[ ] For machine-to-machine authentication (OAuth Client Credentials, JWT, OAuth JWT Bearer), treat filtering as mandatory unless every logged-in user is permitted to read every returned record

## Next Steps

* [Response Transformation](response-transformation) — Template syntax for reshaping response bodies
* [Passing User Context](passing-user-context) — Protect the request side from tampering
* [Headers & Query Parameters](headers-and-query-parameters) — Configure non-overridable filter parameters
* [Testing & Debugging](testing-and-debugging) — Verify what the external API actually returns
