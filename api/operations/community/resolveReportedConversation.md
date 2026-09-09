---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedConversation.md
---

# Resolve reported conversation

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/{id}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `resolvedBy` | query | string | Yes | ID of the moderator resolving reported conversation |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported conversation was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
