---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getTopicListForCategory.md
---

# List topics for a category

CategoriesTopics

List and filter topics for a specific category. Optionally apply some filtering/sorting to find topics matching specific criteria inside this category

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories/{id}/topics
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the category to fetch topics for |
| `publicIds[]` | query | string | No | A list of public ids |
| `tags` | query | string | No | A comma-separated list of public tags |
| `moderatorTags` | query | string | No | A comma-separated list of moderator tags |
| `moderationLabels` | query | string | No | A comma-separated list of moderation labels |
| `createdAt` | query | object | No | A date range to filter topics based on creation date |
| `lastActivityAt` | query | object | No | A date range to filter topics based on last activity |
| `sort` | query | string | No | Defines the field to sort the results on. The sort order will be descending. By default the results are ordered by recent activity from most recent to least recent |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
