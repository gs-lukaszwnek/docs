---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/getAttendeeList.md
---

# Find event attendees by event ID

Events

Fetches a paginated list of attendees for event sorted by signed up date in descending order

## Endpoint

```
GET https://api2-eu-west-1.insided.com/events/events/{id}/attendees
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `X-Prioritized-Attendee` | header | integer | No | Prioritized attendee to be on the first page. |
| `id` | path | string | Yes | ID of the event to interact with |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
