---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/answerQuestion.md
---

# Mark a reply to a question as the answer

Questions

The author who originally asked the question or any moderator can mark a reply that resolves the question as answer.   A question can only have one answer. Note, as the question receives more replies the answer can be updated by the author (or any moderator) who considers another reply to be a more suitable answer.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/answer
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator marking the answer |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply has been marked as answer |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
