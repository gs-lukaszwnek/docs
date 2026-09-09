---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleArticleTrashed.md
---

# Trash or restore an article

Articles

A moderator can trash an article.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing or restoring the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article was trashed |
| 400 | Malformed input |
| 404 | Item not found |
| 500 | Unexpected error |
