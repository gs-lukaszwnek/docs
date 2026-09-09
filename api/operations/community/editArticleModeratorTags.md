---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticleModeratorTags.md
---

# Tag an article with moderator tags

Articles

A moderator can edit moderator tags for an article. Note that this will overwrite existing moderator tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editModeratorTags
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the moderator tags |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `moderatorTags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
