---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/publishArticle.md
---

# Publish an article

Articles

A valid `moderatorId` value is required to publish an article.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/publish
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator publishing the article |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Article published |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
