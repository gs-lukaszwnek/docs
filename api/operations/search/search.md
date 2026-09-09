---
url: https://developer-portal.gainsight.com/docs/api/operations/search/search.md
---

# Search for content in the community

Search

Endpoint to search for a term in the content. The search algorithm is the same as the one in the user facing frontend. Additional filters can be passed to this endpoint in order to narrow down the search results.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/search/search
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `q` | query | string | Yes | Search query |
| `categoryIds` | query | array | No | Category ids to search in |
| `sections` | query | array | No | Sections (categories.lvl0) to search in |
| `parentCategories` | query | array | No | Parent categories (categories.lvl1) to search in |
| `contentTypes` | query | array | No | Content types to search in |
| `tags` | query | array | No | Search in topic with specific tags |
| `moderatorTags` | query | array | No | Search in topic with specific moderator tags |
| `hasAnswer` | query | boolean | No | Set to true to only return questions with answers |
| `page` | query | integer | No | Used for pagination. An integer greated than 0. |
| `pageSize` | query | integer | No | Number of results to return per page (1-200, defaults to 50). |

### Responses

| Status | Description |
|--------|-------------|
| 200 | OK |
| 400 | Validation error |
