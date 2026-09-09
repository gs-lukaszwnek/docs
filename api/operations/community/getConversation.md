---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getConversation.md
---

# Find conversation by ID

Conversations

By default returns a visible conversation. To fetch a trashed conversation a valid `moderatorId` value is required.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/conversations/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `moderatorId` | query | string | No | ID of the moderator fetching the conversation |
| `resolveEmbeds` | query | boolean | No | When set to true, resolves oembed URLs in the content and replaces them with embed HTML |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
