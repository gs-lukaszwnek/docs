---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleConversationClosed.md
---

# Open/Close a conversation

Conversations

A moderator can open or close a conversation for replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/toggleClosed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the closed state for this conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `closed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation closed state was updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
