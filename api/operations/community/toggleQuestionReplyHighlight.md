---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleQuestionReplyHighlight.md
---

# Mark replies as highlighted or remove the highlight

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/toggleHighlight
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator updating the reply |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `highlighted` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply highlight was updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
