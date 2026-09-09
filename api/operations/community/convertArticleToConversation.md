---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/convertArticleToConversation.md
---

# Convert an article to a conversation

Articles

A moderator can convert an article to a conversation. If the article has replies, they are converted to conversation replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/convertToConversation
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Conversion in progress |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
