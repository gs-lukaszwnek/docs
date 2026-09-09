---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/createProductArea.md
---

# Create ProductArea

ProductAreas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productAreas/create
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author of the ProductArea |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | — |
| `parentId` | string | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | ProductArea created |
| 422 | Validation error |
| 500 | Unexpected error |
