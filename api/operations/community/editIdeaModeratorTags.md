---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editIdeaModeratorTags.md
---

# Tag an idea with moderator tags

Ideas

A moderator can edit moderator tags for an idea. Note that this will overwrite existing moderator tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/editModeratorTags
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the moderator tags |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `moderatorTags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea moderator tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
