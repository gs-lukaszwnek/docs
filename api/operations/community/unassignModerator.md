---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/unassignModerator.md
---

# Unassign a article or articles

Articles

A moderator can unassign a article or articles.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/unassignModerator
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator to unassign the article |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `articleIds` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | The article was unassigned |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
