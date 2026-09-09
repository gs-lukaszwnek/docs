---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/renameProductArea.md
---

# Rename ProductArea

ProductAreas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productAreas/rename
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | The ID of the moderator renaming ProductArea |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | — |
| `name` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | ProductArea renamed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
