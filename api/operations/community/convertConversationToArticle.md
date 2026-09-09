---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/convertConversationToArticle.md
---

# Convert a conversation to an article

Conversations

A moderator can convert a conversation to an article. If the conversation has replies, they are converted to article replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/convertToArticle
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the conversation |
| `id` | path | string | Yes | ID of the conversation to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Conversion in progress |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
