---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleIdeaClosed.md
---

# Open/Close an idea

Ideas

A moderator can open or close an idea for replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/toggleClosed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the closed state for this idea |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `closed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea closed state was updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
