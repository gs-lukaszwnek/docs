---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/permanentlyDeleteProductUpdate.md
---

# Permanently delete a trashed productUpdate

ProductUpdates

A moderator can permanently deleted a trashed productUpdate. Warning: once permanently deleted, a productUpdate cannot be restored.

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/productUpdates/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator permanently deleting the productUpdate |
| `id` | path | string | Yes | ID of the product update to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate permanently deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
