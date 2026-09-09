---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/deleteIdeaStatus.md
---

# Permanently delete an idea status

Ideas

A moderator can permanently deleted an idea status. Warning: once permanently deleted, an idea status cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/ideas/{id}/deleteIdeaStatus
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the idea status |
| `id` | path | string | Yes | Idea Status Id to be edited |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea Status permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
