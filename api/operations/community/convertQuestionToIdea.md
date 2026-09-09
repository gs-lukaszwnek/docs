---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/convertQuestionToIdea.md
---

# Convert a question to an idea

Questions

A moderator can convert a question to an idea. If the question has replies, they are converted to idea replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/convertToIdea
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the question |
| `id` | path | string | Yes | ID of the question to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Conversion in progress |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
