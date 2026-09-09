---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedIdeaReply.md
---

# Resolve reported idea reply

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/replies/{replyId}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea reply to interact with |
| `reportedBy` | query | string | Yes | ID of the moderator resolving the reported idea reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported idea reply was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
