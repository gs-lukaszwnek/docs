---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateFeaturedImage.md
---

# Edit the featured image of a productUpdate

ProductUpdates

A moderator can edit a featured image for a productUpdate. Note that this will overwrite existing featured image.  The allowed image extensions for featured image are `png`, `jpeg`, `jpg`, `gif`.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editFeaturedImage
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the featured image |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `featuredImage` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate featured image was changed |
| 422 | Validation error |
| 500 | Unexpected error |
