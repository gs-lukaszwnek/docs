---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markIdeaAsNotSpam.md
---

# Mark idea as not spam

Ideas

Allows moderators to correct false spam statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/markAsNotSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea marked as not spam |
| 422 | Validation error |
| 500 | Unexpected error |
