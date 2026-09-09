---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleConversationTrashed.md
---

# Trash or restore a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing/restoring the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation trashed state updated |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
