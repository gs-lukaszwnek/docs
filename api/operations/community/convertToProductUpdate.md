---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/convertToProductUpdate.md
---

# Convert an article to a product update

Articles

A moderator can convert an article to a product update. If the article has replies, they are converted to product update replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/convertToProductUpdate
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Conversion in progress |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
