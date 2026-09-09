---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/likeQuestionReply.md
---

# Like a reply

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/like
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `likedBy` | query | string | Yes | ID the author giving the like |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply was liked |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
