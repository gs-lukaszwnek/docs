---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateFeaturedImageAltText.md
---

# Edit the featured image alt text of a productUpdate

ProductUpdates

A moderator can edit the featured image alt text for a productUpdate. Note that this will overwrite existing featured image alt text.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editFeaturedImageAltText
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the featured image alt text |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `featuredImageAltText` | string | Yes | Alternative text description for the featured image |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate featured image alt text was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
