---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/listEventWebhookSubscriptions.md
---

# List of Events Webhook subscriptions for a given event.

Webhooks

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/webhooks
```

**Required scope:** `read`

### Responses

| Status | Description |
|--------|-------------|
| 200 | List of EventWebhook subscriptions. |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
