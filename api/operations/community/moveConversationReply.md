---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveConversationReply.md
---

# Move reply to another topic

Conversations

A reply can be moved to an existing article, question or conversation. The topic id and topic type are required in the request body.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{id}/move
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator moving the reply |
| `id` | path | string | Yes | Reply ID |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `topicId` | string | Yes | — |
| `topicType` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Reply moved |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
