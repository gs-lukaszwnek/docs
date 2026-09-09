---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addArticleModeratorTags.md
---

# Add moderator tags to an article

Articles

Adds one or more moderator tags without replacing existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/moderator-tags/add
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the article tags |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
