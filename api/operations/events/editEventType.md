---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventType.md
---

# Change the eventType for a event.

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/changeType
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event type |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `eventTypeName` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event Type updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
