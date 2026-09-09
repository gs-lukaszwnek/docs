---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unvoteIdea.md
---

# Unvote an idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/unvote
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `unvotedBy` | query | string | Yes | ID of the author revoking the vote |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Vote was revoked from the idea |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
