---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/copyConversation.md
---

# Copy a conversation to a category

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/copy
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator copying the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `categoryIds` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Conversation was copied |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
