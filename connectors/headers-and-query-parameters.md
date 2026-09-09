---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/headers-and-query-parameters.md
description: Add custom HTTP headers and query parameters to connector requests
---

# Headers & Query Parameters

Headers and query parameters let you attach fixed or dynamic values to every connector request. Both support [Jinja2](https://jinja.palletsprojects.com/) templates so you can inject user context, secrets, or other runtime values.

## Headers

Add one or more header pairs using **Header Key** and **Header Value**. Use this for things like content type, API versioning, or auth headers.

Example values:

* **Header Key**: `Content-Type` → **Header Value**: `application/json`
* **Header Key**: `Accept` → **Header Value**: `application/vnd.api+json;version=1`

## Query parameters

Add one or more parameter pairs using **Query Key** and **Query Value**. Use this for filters, search terms, pagination, or API keys.

Example values:

* **Query Key**: `q` → **Query Value**:

```jinja2
{{ user.email }}
```

* **Query Key**: `limit` → **Query Value**: `50`

## Values with spaces or special characters

If you put a value with spaces or special characters directly in the connector **URL**, the connector fails to save. The fix: move that value out of the URL and into a query parameter. The platform encodes query-parameter **values** automatically (spaces and characters like `=`, `&`, and quotes), so you never URL-encode them yourself. Auto-encoding applies to query-parameter values, not to the URL field itself.

For example, to send a search query such as `select id from Account where Name = 'Acme Corp'`:

| Approach | Result |
|----------|--------|
| Inline in the URL — `https://api.example.com/query?q=select id from Account...` *(don't do this)* | Rejected — the connector won't save |
| **Query Key** `q` → **Query Value** `select id from Account where Name = 'Acme Corp'` | Works — the value is encoded automatically |

This applies wherever you configure query parameters, including the `query_parameters` array in [Build a Composite Connector](composite-connectors).

## Overridable fields

Each header or query parameter has an **Overridable** checkbox. By default it is off, which means the value is always included in every request and cannot be changed at run time. Turn **Overridable** on when the caller should be able to change the value for a specific request.

When a field is marked as Overridable and no override is provided at run time, the configured value is used as the default. The field is still sent — it is never omitted just because no override was given.

::: warning Identity fields should not be overridable
If a query parameter or header carries user identity (such as a user ID or email), keep **Overridable** off and set the value to a server-side template variable like {{ user.id }}. This ensures the value is injected on the server and cannot be changed by browser code. See [Passing User Context](passing-user-context) for details.
:::

## When to use path parameters instead

Query parameters live in the URL's query string (`?key=value`). For values that go in the URL **path** itself — for example, `/users/{id}/posts` where `{id}` varies per call — declare a **path parameter** instead. Each is configured in its own section of the connector; query-parameter entries are appended to the query string, while path-parameter values are substituted into {{ pathParams.X }} template references in the URL path.

See [Dynamic URL Path Segments](dynamic-url-paths) for the full pattern.

## Next Steps

* [Dynamic URL Path Segments](dynamic-url-paths) — For values that belong in the URL path, not the query string
* [Testing & Debugging](testing-and-debugging) — Verify headers and parameters are sent correctly
* [Template Variables](template-variables) — Jinja2 syntax for dynamic values
* [Authentication](authentication) — Add auth credentials alongside headers
