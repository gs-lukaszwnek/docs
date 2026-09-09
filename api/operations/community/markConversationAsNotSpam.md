---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markConversationAsNotSpam.md
---

# Mark conversation as not spam

Conversations

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
