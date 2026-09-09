---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unsetArticleModerationLabel.md
---

# Unset moderation label from article

Articles

A moderator can unset the moderation label from article.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/unsetModerationLabel
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
| `articleIds` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was unset from article |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
