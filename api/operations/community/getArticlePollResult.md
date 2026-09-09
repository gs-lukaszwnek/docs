---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getArticlePollResult.md
---

# Show poll results

Articles

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/articles/{id}/poll
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article to fetch the poll results for |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
