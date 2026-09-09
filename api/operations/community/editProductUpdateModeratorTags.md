---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateModeratorTags.md
---

# Tag a productUpdate with moderator tags

ProductUpdates

A moderator can edit moderator tags for a productUpdate. Note that this will overwrite existing moderator tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editModeratorTags
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the moderator tags |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `moderatorTags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
