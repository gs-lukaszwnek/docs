---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/setIdeaModerationLabel.md
---

# Set moderation label to idea

Ideas

A moderator can set the moderation label to idea.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/setModerationLabel
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
| `ideaIds` | array of string | Yes | — |
| `moderationLabelId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was set to idea |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
