---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticleFeaturedImageAltText.md
---

# Edit the featured image alt text of an article

Articles

A moderator can edit the featured image alt text for an article. Note that this will overwrite existing featured image alt text.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editFeaturedImageAltText
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the featured image alt text |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `featuredImageAltText` | string | Yes | Alternative text description for the featured image |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article featured image alt text was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
