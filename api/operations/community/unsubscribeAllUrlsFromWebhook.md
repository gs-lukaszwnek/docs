---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unsubscribeAllUrlsFromWebhook.md
---

# Unsubscribe All urls from webhook event

Webhooks

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/webhooks/{eventName}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `eventName` | path | string | Yes | The event name to unsubscribe all urls from. |

### Responses

| Status | Description |
|--------|-------------|
| 204 | URLs unsubscribed |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
