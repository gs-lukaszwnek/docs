---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventTypeName.md
---

# Change the Name for a event Type

Event Types

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/event-types/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event image |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `typeName` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | EventType name updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
