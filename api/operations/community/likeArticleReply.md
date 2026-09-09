---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/likeArticleReply.md
---

# Like a reply

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/replies/{id}/like
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `likedBy` | query | string | Yes | ID the author giving the like |
| `id` | path | string | Yes | Reply ID |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply was liked |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
