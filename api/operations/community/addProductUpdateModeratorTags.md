---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addProductUpdateModeratorTags.md
---

# Add moderator tags to a product update

ProductUpdates

Adds one or more moderator tags without replacing existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/moderator-tags/add
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
