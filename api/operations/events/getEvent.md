---
url: https://developer-portal.gainsight.com/docs/api/operations/events/getEvent.md
---

# Find Event by ID

Events

By default returns a visible event.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/events/events/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the event to interact with |
| `moderatorId` | query | string | No | ID of the moderator viewing the event, needed for viewing draft events. |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
