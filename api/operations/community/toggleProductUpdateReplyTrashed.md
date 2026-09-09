---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleProductUpdateReplyTrashed.md
---

# Trash/restore a reply

ProductUpdates

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/replies/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing/restoring the reply |
| `id` | path | string | Yes | Reply ID |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply trashed state was updated |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
