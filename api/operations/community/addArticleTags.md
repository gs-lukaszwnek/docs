---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addArticleTags.md
---

# Add public tags to an article

Articles

Adds one or more public tags without replacing existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/tags/add
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
