---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedProductUpdateReply.md
---

# Resolve reported product update reply

ProductUpdates

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/replies/{replyId}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the product update reply to interact with |
| `reportedBy` | query | string | Yes | ID of the moderator resolving the reported product update reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported product update reply was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
