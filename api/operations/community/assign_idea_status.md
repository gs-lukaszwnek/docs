---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/assign_idea_status.md
---

# Assign an idea status to an idea

Ideas

A moderator can assign an idea status to an idea. Warning: This replaces the previously assigned status.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/assignIdeaStatus
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator assigning the idea status |
| `id` | path | string | Yes | Idea private ID to be assigned |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `ideaStatusId` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea Status to be assigned |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
