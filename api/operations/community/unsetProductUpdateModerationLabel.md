---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unsetProductUpdateModerationLabel.md
---

# Unset moderation label from product update

ProductUpdates

A moderator can unset the moderation label from product update.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/unsetModerationLabel
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator to unset the moderation label |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `productUpdateIds` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was unset from productUpdate |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
