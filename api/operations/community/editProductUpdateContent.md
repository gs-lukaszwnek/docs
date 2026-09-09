---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdateContent.md
---

# Edit the opening post content of a productUpdate

ProductUpdates

Any moderator can change the content of a productUpdate.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator making the edits |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | ProductUpdate edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
