---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/permanentlyDeleteArticle.md
---

# Permanently delete a trashed article

Articles

A moderator can permanently deleted a trashed article. Warning: once permanently deleted, an article cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/articles/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
