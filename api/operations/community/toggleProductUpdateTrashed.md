---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleProductUpdateTrashed.md
---

# Trash or restore a productUpdate

ProductUpdates

A moderator can trash a productUpdate.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing or restoring the productUpdate |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate was trashed |
| 400 | Malformed input |
| 404 | Item not found |
| 500 | Unexpected error |
