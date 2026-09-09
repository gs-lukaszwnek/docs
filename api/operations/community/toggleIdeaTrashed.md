---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleIdeaTrashed.md
---

# Trash or restore an idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing/restoring the idea |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea trashed state updated |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
