---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markArticleReplyAsNotSpam.md
---

# Mark article reply as not spam

Articles

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/replies/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the article reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article reply marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
