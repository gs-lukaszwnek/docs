---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportConversation.md
---

# Report a conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the conversation |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Conversation was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
