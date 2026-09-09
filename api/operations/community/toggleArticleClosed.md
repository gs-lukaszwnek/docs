---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleArticleClosed.md
---

# Open/Close an article

Articles

A moderator can open or close an article for replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/toggleClosed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the closed state of the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `closed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article closed state was updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
