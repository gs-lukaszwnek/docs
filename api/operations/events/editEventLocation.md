---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventLocation.md
---

# Edit an event location

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/editLocation
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event location |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `location` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event location updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
