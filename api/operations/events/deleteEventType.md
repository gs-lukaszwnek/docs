---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/deleteEventType.md
---

# Delete and EventType

Event Types

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/events/event-types/{id}
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator creating the event |

### Responses

| Status | Description |
|--------|-------------|
| 204 | EventType deleted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
