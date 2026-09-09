---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getPublicTagsList.md
---

# List public tags

Public Tags

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/tags
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `q` | query | string | No | The search term which will match the public tag name. Skipping this parameter will return all public tags |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
