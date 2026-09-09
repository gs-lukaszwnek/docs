---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/mergeIdeaVotes.md
---

# Merge votes into another idea

Ideas

A moderator can merge the votes from one idea into another idea.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/mergeVotes
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | Idea Id to merge the votes from |
| `moderatorId` | query | string | Yes | ID of the moderator to merge the votes |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `toIdeaId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea votes were merged |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
