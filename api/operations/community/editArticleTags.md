---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticleTags.md
---

# Tag an article with tags

Articles

A moderator can edit tags for an article. Note that this will overwrite existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editTags
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author editing the article tags |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
