---
url: https://developer-portal.gainsight.com/docs/api/operations/community/search.md
---

# Search by term across topics

Topics

Perform a full text search on unified topics by passing a search term in the `q` parameter. The search will be performed against topic `title` and `content` fields. It's possible to specify additional optional filters to further limit the results. By default this endpoint will only return visible topics

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/topics/search
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `q` | query | string | No | The search term which will match the title, content. Skipping this parameter will return all topics |
| `trashed` | query | boolean | No | Filter the search results by trashed status, for example to retrieve only trashed topics. Not passing this parameter will return only visible topics |
| `categories` | query | string | No | A comma-separated list of category ids. Not passing this parameter will return topics from all categories |
| `contentTypes` | query | string | No | A comma-separated list of content types |
| `tags` | query | string | No | A comma-separated list of public tags |
| `moderatorTags` | query | string | No | A comma-separated list of moderator tags |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
