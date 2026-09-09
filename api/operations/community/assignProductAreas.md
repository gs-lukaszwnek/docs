---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/assignProductAreas.md
---

# Assign ProductAreas to Ideas

ProductAreas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productAreas/assign
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | The ID of the moderator assigning the ProductAreas to Ideas |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `ideaIds` | string | Yes | — |
| `productAreaIds` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductAreas assigned |
| 422 | Validation error |
| 500 | Unexpected error |
