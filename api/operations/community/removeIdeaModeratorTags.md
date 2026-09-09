---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/removeIdeaModeratorTags.md
---

# Remove moderator tags from an idea

Ideas

Removes one or more moderator tags without affecting other tags on the idea.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/moderator-tags/remove
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the idea tags |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
