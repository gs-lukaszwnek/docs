---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unsetQuestionModerationLabel.md
---

# Unset moderation label from question

Questions

A moderator can unset the moderation label from question.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/unsetModerationLabel
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
| `questionIds` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderation label was unset from question |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
