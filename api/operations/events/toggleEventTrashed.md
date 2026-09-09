---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/toggleEventTrashed.md
---

# Trash an event

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/toggleTrashed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator trashing the event |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `trashed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event trashed state updated |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
