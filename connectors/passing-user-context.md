---
url: https://developer-portal.gainsight.com/docs/connectors/passing-user-context.md
description: >-
  How to securely pass user identity and context to connectors using server-side
  template variables instead of browser-supplied parameters
---

# Passing User Context

Use this guide when your connector needs to identify the logged-in user — for example, to fetch their records from a CRM, personalize content, or enforce access control on an external API.

## Prerequisites

* An existing connector configured in **Integrations** → **Developer Studio** → **Connectors** (see [Build Your First Connector](build-first-connector) if you do not have one yet)
* The external API accepts a user identifier (user ID, email, or similar) as a query parameter, header, or request body field

## Browser data vs server-side variables

Every connector request passes through a [server-side pipeline](how-connectors-work) before reaching the external API. During this pipeline, template variables like {{ user.id }} are resolved from the authenticated session — the browser cannot see or tamper with these values.

Everything the browser sends — query parameters, headers, request bodies — can be modified by the user. A malicious user can open browser dev tools and change `userId=123` to `userId=456` before the request leaves. Server-side template variables like {{ user.id }} are resolved on the backend from the authenticated session, so the user cannot tamper with them.

## Use server-side variables for identity

When a connector needs to know *who* the user is, set the value in the connector's configuration using a template variable. Do not pass it from browser JavaScript.

::: danger Wrong: passing identity from the browser
The browser controls `queryParams`, so a user can change the value to impersonate someone else.

```javascript
// Browser code — user can tamper with the ID
const sdk = new window.WidgetServiceSDK();
const data = await sdk.connectors.execute({
  permalink: "user-profile-api",
  method: "GET",
  queryParams: {
    user_id: currentUser.id  // attacker changes this in dev tools
  }
});
```

:::

::: tip Right: set the variable in connector config
In the connector's **Query Parameters** section, add a non-overridable parameter:

* **Query Key**: `user_id`
* **Query Value**: {{ user.id }}
* **Overridable**: unchecked

The server resolves {{ user.id }} from the authenticated session. The browser never sees or controls this value.
:::

Server-side variables are available in any connector field (URL, headers, query parameters, authentication, payload template, response template) — see [Template Variables](template-variables) for the full reference, including filters, functions, and availability per field.

## When browser parameters are appropriate

Not all data needs server-side protection. The rule is: **anything the user could legitimately change is fine as a `queryParam`; anything that identifies who the user is must come from server-side variables.**

Browser-supplied parameters are appropriate for:

* Search terms (`q=react hooks`)
* Filters (`category=billing`)
* Pagination (`page=3`, `limit=25`)
* Sort order (`sort=created_at&dir=desc`)

These values carry no security risk if the user changes them — they only affect what the user sees, not *whose* data they see.

## The Overridable checkbox

Each header and query parameter has an **Overridable** flag. For identity fields, always leave **Overridable** unchecked — this guarantees the server-side template variable cannot be replaced by browser code. Use **Overridable** only for values the user should legitimately control, like search terms or pagination. See [Headers & Query Parameters](headers-and-query-parameters#overridable-fields) for the full explanation.

## Common patterns

### Pass the logged-in user's ID to an external API

In the connector's **Query Parameters**:

* **Query Key**: `user_id` — **Query Value**: {{ user.id }} — **Overridable**: unchecked

### Pass user email for personalized lookups

In the connector's **Query Parameters**:

* **Query Key**: `email` — **Query Value**: {{ user.email }} — **Overridable**: unchecked

### Pass tenant ID for multi-tenant APIs

In the connector's **Headers**:

* **Header Key**: `X-Tenant-ID` — **Header Value**: {{ tenant\_id }} — **Overridable**: unchecked

### Pass a custom profile field

In the connector's **Query Parameters**:

* **Query Key**: `department` — **Query Value**: {{ user.profile\_fields.by\_id(42) }} — **Overridable**: unchecked

Replace `42` with the numeric ID of the profile field from your community's Profile Fields settings.

## Next Steps

* [Template Variables](template-variables) — Full reference of variables, filters, and functions
* [Headers & Query Parameters](headers-and-query-parameters) — Configure static and overridable values
* [Calling from Widget Code](calling-from-widgets) — Invoke connectors from widget JavaScript
* [Filtering Sensitive Data](filtering-sensitive-data) — Keep hidden or private records out of responses
* [Authentication](authentication) — Secure your connector's external API requests
