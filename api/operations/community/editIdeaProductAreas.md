---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editIdeaProductAreas.md
---

# Attach an idea with product areas

Ideas

A moderator can edit product areas for an idea. Note that this will overwrite existing product areas.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/editProductAreas
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the idea product areas |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `productAreas` | array of string | Yes | A comma-separated list of product area ids the Update refers to |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate product areas were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
