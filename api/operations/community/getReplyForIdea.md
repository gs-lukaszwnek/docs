---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getReplyForIdea.md
---

# Fetch a single reply for an idea

Ideas

By default returns a visible reply. A valid `moderatorId` value is required to fetch a trashed reply.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/ideas/{id}/replies/{replyId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `replyId` | path | string | Yes | ID of the reply to fetch |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
