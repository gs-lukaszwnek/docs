---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/assignModerator.md
---

# Assign a moderator to article or articles

Articles

A moderator can assign another moderator to an article or articles.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/assignModerator
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator to assign the article |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `articleIds` | array of string | Yes | — |
| `assignedTo` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderator was assigned to the article |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
