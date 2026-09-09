---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/approveIdeaReply.md
---

# Approve a reply

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/replies/{replyId}/approve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator approving the reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply was approved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
