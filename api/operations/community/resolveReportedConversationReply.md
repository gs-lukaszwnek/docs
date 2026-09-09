---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedConversationReply.md
---

# Resolve reported conversation reply

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{replyId}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation reply to interact with |
| `resolvedBy` | query | string | Yes | ID of the moderator resolving the reported conversation reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported conversation reply was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
