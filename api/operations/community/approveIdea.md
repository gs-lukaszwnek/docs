---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/approveIdea.md
---

# Approve an idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/approve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator approving the idea |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea was approved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
