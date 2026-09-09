---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addConversationModeratorTags.md
---

# Add moderator tags to a conversation

Conversations

Adds one or more moderator tags without replacing existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/moderator-tags/add
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the conversation tags |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
