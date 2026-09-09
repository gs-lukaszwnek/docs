---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveConversationToTopic.md
---

# Convert a conversation to a reply

Conversations

Move a conversation with no visible replies to an existing topic. It can be converted to a reply of an article, question or another conversation. This action will permanently delete any trashed replies the conversation may have.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/moveToTopic
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `topicId` | string | Yes | — |
| `topicType` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Conversation moved to topic |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
