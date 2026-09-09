---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/search/searchTags.md
---

# Search for tags in the community

Search

Endpoint to search for tags by name. Returns a list of matching tags with their IDs and usage counts.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/search/search/tags
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `q` | query | string | No | Search query for tag name |
| `page` | query | integer | No | Page number for pagination |
| `pageSize` | query | integer | No | Number of results to return per page (1-200, defaults to 50). |

### Responses

| Status | Description |
|--------|-------------|
| 200 | OK |
| 400 | Validation error |
