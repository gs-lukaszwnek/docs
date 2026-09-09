---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/createPublicTag.md
---

# Create public tag

Public Tags

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/tags/create
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author of the tag |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Tag created |
| 422 | Validation error |
| 500 | Unexpected error |
