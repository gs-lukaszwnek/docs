---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/permanentlyDeleteIdea.md
---

# Permanently delete a trashed idea

Ideas

A moderator can permanently deleted a trashed idea. Warning: once permanently deleted, an idea cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/ideas/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the idea |
| `id` | path | string | Yes | ID of the idea to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
