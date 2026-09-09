---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveConversation.md
---

# Move a conversation to a different category

Conversations

A moderator can move a conversation to a different category. The moderator must have access to the both the category in which the conversation currently is as well as the category to which the conversation is to be moved.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/move
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator moving the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `categoryId` | integer | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation was moved |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
