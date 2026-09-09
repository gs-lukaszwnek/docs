---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedQuestionReply.md
---

# Resolve reported question reply

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/replies/{replyId}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `replyId` | path | string | Yes | ID of the question reply to interact with |
| `resolvedBy` | query | string | Yes | ID of the moderator resolving the reported question reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported question reply was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
