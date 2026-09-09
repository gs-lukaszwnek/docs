---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/approveConversation.md
---

# Approve a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/approve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator approving the conversation |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation was approved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
