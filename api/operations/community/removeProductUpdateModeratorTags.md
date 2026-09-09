---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/removeProductUpdateModeratorTags.md
---

# Remove moderator tags from a product update

ProductUpdates

Removes one or more moderator tags without affecting other tags on the product update.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/moderator-tags/remove
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the productUpdate tags |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
