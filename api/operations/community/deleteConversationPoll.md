---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/deleteConversationPoll.md
---

# Delete poll

Conversations

Be cautious when deleting a poll. Once deleted it cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/conversations/{id}/poll
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | Conversation ID |
| `moderatorId` | query | string | Yes | ID of the moderator deleting the poll |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Poll attached to conversation deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
