---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/likeConversation.md
---

# Like a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/like
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `likedBy` | query | string | Yes | ID of the author giving the like |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation was liked |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
