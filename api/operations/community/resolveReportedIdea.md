---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/resolveReportedIdea.md
---

# Resolve reported idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/resolve
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `resolvedBy` | query | string | Yes | ID of the moderator resolving reported idea |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reported idea was resolved |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
