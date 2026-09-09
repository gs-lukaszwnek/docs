---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/replyConversation.md
---

# Reply to a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/reply
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `authorId` | query | string | Yes | The ID of the author of this reply |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |
| `highlight` | boolean | No | Setting this to true will create a highlighted reply. This will require moderator access |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Conversation was replied |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
