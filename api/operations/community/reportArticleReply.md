---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportArticleReply.md
---

# Report an article reply

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/replies/{replyId}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article reply to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the article reply |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article reply was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
