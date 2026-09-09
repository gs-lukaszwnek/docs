---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addProductUpdatePoll.md
---

# Add a poll to a product update

ProductUpdates

A moderator can add a poll to a product update that does not already contain one. Only allowed for draft product updates.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/addPoll
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator adding the poll |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `options` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Poll was added to the product update |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
