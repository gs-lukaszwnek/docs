---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markQuestionReplyAsSpam.md
---

# Mark question reply as spam

Questions

This action automatically bans the author of the content by default. Ensure the content clearly violates spam policies before proceeding.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{id}/markAsSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `replyId` | path | string | Yes | ID of the question reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Request Body

`application/json`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `banUser` | boolean | No | Whether to ban the author of the content. Defaults to true if not specified. |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question reply marked as spam |
| 422 | Validation error |
| 500 | Unexpected error |
