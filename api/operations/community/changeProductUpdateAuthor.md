---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/changeProductUpdateAuthor.md
---

# Change the author of a product update

ProductUpdates

A moderator can change the author of a product update.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/changeAuthor
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator changing the author of the product update |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `authorId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Author has been changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
