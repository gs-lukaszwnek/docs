---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/approveQuestion.md
---

# Approve a question

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/approve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator approving the question |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question was approved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
