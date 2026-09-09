---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getReplyForArticle.md
---

# Fetch a reply for an article

Articles

By default returns a visible reply. A valid `moderatorId` value is required to fetch a trashed reply.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/articles/{id}/replies/{replyId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article to fetch the reply for |
| `replyId` | path | string | Yes | ID of the reply to fetch |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
