---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportQuestionReply.md
---

# Report a question reply

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `replyId` | path | string | Yes | ID of the question reply to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the question reply |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question reply was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
