---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleConversationStickyState.md
---

# Toggle the sticky state of a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/toggleStickyState
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the conversation sticky state |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `sticky` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation sticky state updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
