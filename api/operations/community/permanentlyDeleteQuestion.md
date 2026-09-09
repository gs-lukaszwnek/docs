---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/permanentlyDeleteQuestion.md
---

# Permanently delete a trashed question

Questions

A moderator can permanently deleted a trashed question. Warning: once permanently deleted, a question cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/questions/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the question |
| `id` | path | string | Yes | ID of the question to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
