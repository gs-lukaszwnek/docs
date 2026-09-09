---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addIdeaTags.md
---

# Add public tags to an idea

Ideas

Adds one or more public tags without replacing existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/tags/add
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author editing the idea tags |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
