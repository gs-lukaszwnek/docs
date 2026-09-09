---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleQuestionReplyTrashed.md
---

# Trash/restore a reply

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing/restoring the reply |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply trashed state was updated |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
