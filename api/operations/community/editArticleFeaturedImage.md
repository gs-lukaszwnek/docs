---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticleFeaturedImage.md
---

# Edit the featured image of an article

Articles

A moderator can edit a featured image for an article. Note that this will overwrite existing featured image.  The allowed image extensions for featured image are `png`, `jpeg`, `jpg`, `gif`.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editFeaturedImage
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the featured image |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `featuredImage` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article featured image was changed |
| 422 | Validation error |
| 500 | Unexpected error |
