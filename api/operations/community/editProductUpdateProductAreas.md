---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateProductAreas.md
---

# Attach a productUpdate with product areas

ProductUpdates

A moderator can edit product areas for a productUpdate. Note that this will overwrite existing product areas.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editProductAreas
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the productUpdate product areas |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `productAreas` | array of string | Yes | A comma-separated list of product area ids the Update refers to |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate product areas were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
