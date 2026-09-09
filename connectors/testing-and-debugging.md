---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/testing-and-debugging.md
description: Test connector requests and diagnose failures using the built-in test tool
---

# Testing & Debugging

Use this page to test your connectors during development and diagnose issues in production, including slow responses and failed requests.

## Testing

In the connector editor, click **Test Connection** in the toolbar to send a test request before saving. Use it before running in production to confirm:

* URL connectivity
* Authentication
* Response shape

A successful test displays a `200` response with the JSON body returned by the external API. A failed test displays the error code and message from the destination service, helping you identify the issue before the connector runs in production.

### Debug request header

Every **Test Connection** response includes an `X-Debug-Request` header containing the fully resolved request that was sent to the external API. Open your browser's Developer Tools, go to the **Network** tab, and inspect the response headers to see it.

The header value is a JSON object with these fields:

| Field | Description |
|-------|-------------|
| `url` | The resolved URL after template rendering |
| `method` | HTTP method (`GET`, `POST`, etc.) |
| `headers` | Resolved request headers sent to the external API |
| `query_parameters` | Resolved query parameters |
| `authentication` | Resolved authentication configuration |
| `request_body` | Resolved request body, or `null` if none |
| `response_body` | The response body template string, or `null` if none |
| `response_content_type` | The response content type override, or `null` if none |

**Secret values are never included.** Any field that uses {{ get\_secret('...') }} appears as the original template literal instead of the actual secret value. All other templates — such as {{ user.email }} or {{ now() }} — show their resolved values.

Use this header to verify:

* Your URL and query parameters resolved correctly
* Authentication claims (such as JWT `iss`, `iat`, `exp`) contain the expected values
* Headers include the right content type and authorization scheme
* Secret references are in the right places without exposing actual credentials

::: tip
The debug header appears on both successful and failed test responses, so you can inspect the resolved request even when the external API returns an error.
:::

## Troubleshooting

### Connection issues

* In the connector editor, verify the **URL** starts with `https://` and points to the correct host and path. A missing path segment or trailing slash can change which endpoint you reach.
* Click **Test Connection** to confirm the destination is reachable. If the test fails with a network error, confirm the external service is online and that it accepts requests from external clients.
* Check for typos in the hostname. DNS resolution failures and "connection refused" errors often come from a misspelled domain.

### Authentication issues

* Open the **Secrets** page and confirm the secret names used in your connector match exactly, including case and any prefix characters.
* If the connector uses OAuth, verify the **Token URL** and **scopes** in the connector editor match what the external API expects. An incorrect scope often returns a `403 Forbidden` rather than an authentication error.
* Click **Test Connection** after updating credentials. Expired or rotated tokens are a common cause of authentication failures that worked previously.

### Template issues

* In the connector editor, confirm every variable name in your payload template matches a property available in the request context. A misspelled variable renders as an empty string, which can cause the external API to reject the request.
* If a template references a secret with `get_secret()`, open the **Secrets** page and verify the secret exists and its name matches the function argument exactly.
* Click **Test Connection** and check the `X-Debug-Request` response header in your browser's Developer Tools. It shows exactly how each template resolved — compare the rendered values against what the external API expects. See [Debug request header](#debug-request-header) for details.

### Secrets and Variables issues

* If a template references a Secret or Variable that does not exist, or the name is misspelled, `get_secret()` and `get_variable()` leave the original template expression unresolved in the rendered output, and that literal text is then used in the request. A missing Variable in the URL causes the URL to fail validation. A missing Secret in a header is sent to the external API as literal text, which typically returns a `401 Unauthorized` or `403 Forbidden`.
* Click **Test Connection** and inspect the `X-Debug-Request` header. If a field shows the raw `get_secret('...')` or `get_variable('...')` text instead of a resolved value, the name did not resolve. See [Debug request header](#debug-request-header).
* Open **Integrations** → **Developer Studio** → **Secrets and Variables** and confirm the entry exists with exactly the name you used, then compare it character-by-character with the name in `get_secret('name')` or `get_variable('name')` — names are case-sensitive.
* If a save is rejected when creating a Secret or Variable: the name may contain hyphens, spaces, or special characters (see [Secrets and Variables](secrets/#naming-rules)); the value may be empty; or an entry with that name may already exist — edit the existing entry or choose a different name.
* **Secret value looks wrong** — the masked display only shows the first character. Edit the secret and re-enter the value to confirm it is correct.
* **Credentials stopped working** — some API keys and tokens expire. Edit the secret with fresh credentials, then re-test the connector.
* **Changing a Secret to a Variable (or the reverse)** — the type is set when you create an entry and cannot be changed afterward. Delete the entry and add it again with the type you want.
* **Template formatting** — use single quotes: `get_variable('name')`, not `get_variable("name")`. Avoid extra spaces: `get_variable('api_environment')`, not `get_variable( 'api_environment' )`.

## Debugging connector performance

When connectors run slowly, you can use built-in performance headers to identify the root cause. These headers are included in every connector execution response.

### Performance headers

Every connector response includes timing information in HTTP headers:

**X-Request-ID**: A unique identifier for correlating requests with logs and your Gainsight team.

```
X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890
```

**Server-Timing**: W3C standard timing breakdown showing where time was spent.

```
Server-Timing: setup;dur=5.1;desc="Setup", transform;dur=10.3;desc="Template",
               cache;dur=2.0;desc="Cache Miss", external;dur=200.5;desc="api.stripe.com",
               total;dur=245.7;desc="Total"
```

**X-Connector-Timing**: Detailed JSON breakdown for programmatic analysis. The base structure includes timing metrics that are always present:

```json
{
  "request_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "total_ms": 245.7,
  "internal_ms": 45.2,
  "external_ms": 200.5,
  "breakdown": {
    "setup_ms": 5.1,
    "transform_ms": 10.3,
    "cache_ms": 2.0,
    "http_request_ms": 200.5
  },
  "cache_hit": false
}
```

The following fields appear only when relevant:

| Field | Present when | Example value |
|-------|-------------|---------------|
| `external_host` | The request reached an external API | `"api.stripe.com"` |
| `timeout` | The external request timed out | `true` |
| `error` | An error occurred during execution | `"Connection refused"` |

For example, a request that timed out includes both `timeout` and `external_host`:

```json
{
  "request_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "total_ms": 15045.2,
  "internal_ms": 45.2,
  "external_ms": 15000.0,
  "breakdown": {
    "setup_ms": 5.1,
    "transform_ms": 10.3,
    "cache_ms": 2.0,
    "http_request_ms": 15000.0
  },
  "cache_hit": false,
  "external_host": "api.stripe.com",
  "timeout": true
}
```

### Reading timing headers in browser DevTools

1. Open your browser's Developer Tools (F12 or Cmd+Option+I)
2. Go to the Network tab
3. Trigger a connector request from your widget or the browser console
4. Click on the connector request
5. Look at the Response Headers section for timing information

### Understanding the timing breakdown

**Top-level fields:**

| Metric | Description | What it tells you |
|--------|-------------|-------------------|
| `internal_ms` | Time spent in the platform | Platform processing overhead |
| `external_ms` | Time spent calling external APIs | Third-party API latency |

**`breakdown` sub-fields:**

| Metric | Description | What it tells you |
|--------|-------------|-------------------|
| `setup_ms` | Connector initialization | Configuration loading time |
| `transform_ms` | Template rendering | [Jinja2](https://jinja.palletsprojects.com/) template processing |
| `cache_ms` | Cache lookup duration | Cache operation overhead |
| `http_request_ms` | External HTTP request | Time waiting for external API |

### Troubleshooting slow connectors

**If `external_ms` is high (>500ms):**

* The issue is with your third-party API, not the platform
* Check the external service's status page
* Consider if the API has rate limits or throttling
* Review the API's performance documentation
* Contact the third-party API provider for support

**If `internal_ms` is high (>100ms):**

* Template rendering may be complex -- check `transform_ms` in the breakdown
* Talk to your Gainsight team with your X-Request-ID for analysis

**If the `Server-Timing` header shows `cache;desc="Cache Miss"` frequently:**

* Responses are not being served from cache
* Check if the external API supports HTTP caching headers
* Consider whether real-time data is actually required for your use case

### Using X-Request-ID for support

When talking to your Gainsight team about a connector issue, include the following:

1. Copy the `X-Request-ID` from the response headers
2. Note the approximate time of the issue
3. Include the connector name and any error messages
4. Your Gainsight team can use the request ID to find detailed traces and logs

Example support message:

> "My 'Salesforce Contact Lookup' connector is timing out.
> Request ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890
> Time: 2024-05-15 14:30 UTC
> Server-Timing shows external;dur=15000;desc="Timeout""

## Getting help

If you have worked through the troubleshooting steps above and the issue persists, talk to your Gainsight team. Include your `X-Request-ID`, the connector name, and a description of the behavior you see. Your Gainsight team can use the request ID to trace the full request lifecycle and identify the root cause. Common issues they can help with include rate limits, API version mismatches, and regional endpoint differences.

## Next Steps

* [Secrets and Variables](secrets/) — Store credentials and configuration values referenced from connectors
* [HTTP Caching](http-caching) — Reduce latency with automatic response caching
