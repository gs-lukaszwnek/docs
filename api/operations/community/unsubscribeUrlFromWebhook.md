---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unsubscribeUrlFromWebhook.md
---

# Unsubscribe url from webhook event

Webhooks

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/webhooks/{eventName}/subscriptions
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `eventName` | path | string | Yes | The event name to unsubscribe from. |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `url` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | URL unsubscribed |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
