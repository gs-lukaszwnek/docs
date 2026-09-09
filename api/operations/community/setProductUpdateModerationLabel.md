---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/setProductUpdateModerationLabel.md
---

# Set moderation label to product update

ProductUpdates

A moderator can set the moderation label to product update.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/setModerationLabel
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator to set the moderation label |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `productUpdateIds` | array of string | Yes | — |
| `moderationLabelId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was set to productUpdate |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
