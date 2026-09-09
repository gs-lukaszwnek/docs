---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/publishEvent.md
---

# Publish an event

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/publish
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator publishing the event |
| `id` | path | string | Yes | ID of the event to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event published. |
| 400 | Malformed input |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
