---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reorderIdeaStatuses.md
---

# Reorder idea statuses

Ideas

A moderator can reorder Idea statuses.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/reorderIdeaStatuses
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `order` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Idea Statuses reordered |
| 422 | Validation error |
| 500 | Unexpected error |
