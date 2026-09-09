---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unlikeConversation.md
---

# Unlike a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/unlike
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `unlikedBy` | query | string | Yes | ID of the author revoking the like |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Like was revoked from the discussion |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
