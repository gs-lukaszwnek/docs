---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/removeQuestionAnswer.md
---

# Remove an answer from a question

Questions

The author who originally asked the question or any moderator can remove an answer from a question.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/answer
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author or moderator removing the reply. |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Answer removed from question |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
