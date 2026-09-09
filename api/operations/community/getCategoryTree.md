---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getCategoryTree.md
---

# Get categories tree

Categories

Returns a hierarchical tree structure of categories filtered by module(s). The response is organized by category module, with each category containing its nested children recursively. Categories are sorted by displayOrder. If topLevelSectionIds is provided, only the specified top-level sections are returned (this filter is only available when querying a single category module).

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/category/getTree
```

**Required scope:** `read`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `module` | query | array of string | Yes | Array of category modules to retrieve. Valid values: community, knowledge-base, groups. Example: ?module\[]=community\&module\[]=groups |
| `topLevelSectionIds` | query | array of integer | No | Optional array of top-level section IDs to filter by. Only available when querying a single module. Example: ?topLevelSectionIds\[]=1\&topLevelSectionIds\[]=2 |
| `authorId` | query | string | Yes | The ID of the author making the request |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Categories tree retrieved successfully |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
