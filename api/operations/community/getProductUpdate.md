---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getProductUpdate.md
---

# Find productUpdate by ID

ProductUpdates

To fetch a trashed productUpdate a valid `moderatorId` value is required.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/productUpdates/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the productUpdate to fetch |
| `moderatorId` | query | string | No | ID of the moderator fetching the productUpdate |
| `resolveEmbeds` | query | boolean | No | When set to true, resolves oembed URLs in the content and replaces them with embed HTML |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
