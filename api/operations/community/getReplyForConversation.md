---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getReplyForConversation.md
---

# Fetch a single reply for a conversation

Conversations

By default returns a visible reply. A valid `moderatorId` value is required to fetch a trashed reply.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/conversations/{id}/replies/{replyId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `replyId` | path | string | Yes | ID of the reply to fetch |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
