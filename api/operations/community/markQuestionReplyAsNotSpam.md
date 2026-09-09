---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markQuestionReplyAsNotSpam.md
---

# Mark question reply as not spam

Questions

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `replyId` | path | string | Yes | ID of the question reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question reply marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
