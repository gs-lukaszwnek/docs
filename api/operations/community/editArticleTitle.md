---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticleTitle.md
---

# Edit the title of an article

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editTitle
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator making the edits |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Article edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
