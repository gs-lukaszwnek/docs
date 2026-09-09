---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getArticle.md
---

# Find article by ID

Articles

To fetch a trashed article a valid `moderatorId` value is required.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/articles/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article to fetch |
| `moderatorId` | query | string | No | ID of the moderator fetching the article |
| `resolveEmbeds` | query | boolean | No | When set to true, resolves oembed URLs in the content and replaces them with embed HTML |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
