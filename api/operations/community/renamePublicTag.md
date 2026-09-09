---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/renamePublicTag.md
---

# Rename public tag

Public Tags

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/tags/rename
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | The ID of the moderator renaming tag |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | — |
| `name` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Tag renamed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
