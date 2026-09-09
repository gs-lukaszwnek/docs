---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getTopicList.md
---

# List topics

Topics

List and filter all topics. Optionally apply some filtering/sorting to find topics matching specific criteria. It's possible to list topics from all categories, or filtered on specific categories. Only the first 10,000 topics matching the filter can be queried, requests for further topics will return a 422 error. *IMPORTANT NOTES* : 1) using parameters like categoryId and categoryIds simultaneously leads to their merging 2) using categoryId(s) and productAreaIds simultaneously gives OR effect

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/topics
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `q` | query | string | No | The search term which will match the title, content. Skipping this parameter will return all topics |
| `publicIds[]` | query | string | No | A list of public ids |
| `categoryId` | query | string | No | Deprecated it's recommended to use categoryIds |
| `categoryIds[]` | query | string | No | A list of category ids |
| `productAreaIds[]` | query | string | No | A list of product area ids |
| `contentType` | query | string | No | Deprecated, it's recommended to use contentTypes |
| `contentTypes[]` | query | string | No | A list of topic content types |
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
