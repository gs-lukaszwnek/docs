---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportProductUpdateReply.md
---

# Report a product update reply

ProductUpdates

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/replies/{replyId}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the product update reply to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the product update reply |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Product update reply was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
