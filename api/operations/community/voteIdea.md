---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/voteIdea.md
---

# Vote an idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/vote
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `votedBy` | query | string | Yes | ID of the author giving the vote |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea was voted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
