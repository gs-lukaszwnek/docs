---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markProductUpdateReplyAsNotSpam.md
---

# Mark productUpdate reply as not spam

ProductUpdates

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/replies/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the product update reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate reply marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
