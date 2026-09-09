---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editConversationContent.md
---

# Edit the opening post content of a conversation

Conversations

An author can change the content of a conversation for a limited time after starting the conversation. The default edit time span is 60 minutes.  Moderators can always change the content of a conversation that was started by any other author.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator making the edits |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Conversation edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
