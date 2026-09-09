---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/rescheduleEvent.md
---

# Reschedule an event

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/reschedule
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator rescheduling the event |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `startsAt` | string | Yes | ISO format date-time representing the start date and time of the event |
| `endsAt` | string | Yes | ISO format date-time representing the end date and time of the event |
| `timezone` | string | Yes | A valid timezone name from the IANA database. For reference: [list of IANA timezone names](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event rescheduled |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
