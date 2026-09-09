---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getQuestionReply.md
---

# Fetch a single reply for a question

Questions

By default returns a visible reply. A valid `moderatorId` value is required to fetch a trashed reply.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/questions/{id}/replies/{replyId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `replyId` | path | string | Yes | ID of the question reply to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
