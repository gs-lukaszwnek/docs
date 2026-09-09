---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editConversationTitle.md
---

# Edit the title of a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/editTitle
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the conversation title |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation title was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
