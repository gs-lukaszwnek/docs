---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/setArticleModerationLabel.md
---

# Set moderation label to article

Articles

A moderator can set the moderation label to article.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/setModerationLabel
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
| `articleIds` | array of string | Yes | — |
| `moderationLabelId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was set to article |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
