---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedQuestion.md
---

# Resolve reported question

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `resolvedBy` | query | string | Yes | ID of the moderator resolving reported question |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported question was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
