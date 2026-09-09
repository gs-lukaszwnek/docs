---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/permanentlyDeleteConversation.md
---

# Permanently delete a trashed conversation

Conversations

A moderator can permanently deleted a trashed conversation. Warning: once permanently deleted, a conversation cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/conversations/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
