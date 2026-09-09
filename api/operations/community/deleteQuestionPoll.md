---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/deleteQuestionPoll.md
---

# Delete poll attached to a question

Questions

Be cautious when deleting a poll. Once deleted it cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/questions/{id}/poll
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator deleting the question poll |
| `id` | path | string | Yes | ID of the question to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Poll attached to question deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
