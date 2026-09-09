---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveProductUpdateReply.md
---

# Move an existing reply to an existing topic

ProductUpdates

A reply can be moved to an existing productUpdate.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/replies/{id}/move
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator moving the reply |
| `id` | path | string | Yes | Reply ID |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `topicId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Reply moved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
