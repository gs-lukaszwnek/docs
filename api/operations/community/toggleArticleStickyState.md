---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleArticleStickyState.md
---

# Toggle the sticky state of an article

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/toggleStickyState
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the article sticky state |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `sticky` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article sticky state updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
