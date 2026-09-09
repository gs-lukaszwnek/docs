---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markIdeaReplyAsNotSpam.md
---

# Mark idea reply as not spam

Ideas

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/replies/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea reply to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea reply marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
