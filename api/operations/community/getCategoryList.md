---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getCategoryList.md
---

# List categories

Categories

Fetches a paginated list of categories. It returns only public categories if the authorId is not specified. The result doesn't project a tree view and the result is sorted by display order of the category in ascending order (Display order starts from 0)

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |
| `authorId` | query | string | No | ID of the user fetching the categories (required if wanting to see non-public categories) |
| `excludeGroups` | query | boolean | No | When true, excludes group entries (public, private and hidden groups) from the response so only categories are returned. Defaults to false if not specified. |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
