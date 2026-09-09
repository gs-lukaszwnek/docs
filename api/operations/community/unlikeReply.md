---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unlikeReply.md
---

# Unlike a reply

Conversations

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{id}/unlike
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `unlikedBy` | query | string | Yes | ID of the author who unlikes the reply |
| `id` | path | string | Yes | Reply ID |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Like was revoked from the reply |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
