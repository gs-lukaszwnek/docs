---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/removeProductUpdateTags.md
---

# Remove public tags from a product update

ProductUpdates

Removes one or more public tags without affecting other tags on the product update.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/tags/remove
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author editing the productUpdate tags |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
