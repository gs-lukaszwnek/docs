---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/likeQuestion.md
---

# Like a question

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/like
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `likedBy` | query | string | Yes | ID the author giving the like |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question was liked |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
