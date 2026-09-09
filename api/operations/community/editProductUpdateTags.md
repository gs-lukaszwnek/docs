---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateTags.md
---

# Tag a productUpdate with tags

ProductUpdates

A moderator can edit tags for a productUpdate. Note that this will overwrite existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editTags
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
