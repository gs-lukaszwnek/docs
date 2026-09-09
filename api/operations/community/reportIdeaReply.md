---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportIdeaReply.md
---

# Report an idea reply

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/replies/{replyId}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea reply to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the idea reply |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea reply was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
