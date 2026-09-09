---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/deleteProductArea.md
---

# Delete ProductArea

ProductAreas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productAreas/delete
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | The ID of the moderator deleting ProductArea |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductArea deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
