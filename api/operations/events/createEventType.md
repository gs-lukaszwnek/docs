---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/createEventType.md
---

# Create an EventType

Event Types

## Endpoint

```
PUT https://api2-eu-west-1.insided.com/events/event-types
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator creating the event |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `typeName` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | EventType created |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
