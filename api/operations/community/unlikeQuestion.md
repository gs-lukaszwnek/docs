---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unlikeQuestion.md
---

# Unlike a question

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/unlike
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `unlikedBy` | query | string | Yes | ID the author revoking the like |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Like was revoked from the question |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
