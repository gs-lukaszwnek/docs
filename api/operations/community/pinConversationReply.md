---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/pinConversationReply.md
---

# Pin a reply

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{id}/pin
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | Reply ID |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply was pinned |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
