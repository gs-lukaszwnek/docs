---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedArticleReply.md
---

# Resolve reported article reply

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/replies/{replyId}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article reply to interact with |
| `reportedBy` | query | string | Yes | ID of the moderator resolving the reported article reply |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported article reply was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
