---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/getEventTypesList.md
---

# List event types

Event Types

Fetches a paginated list of event types

## Endpoint

```
GET https://api2-eu-west-1.insided.com/events/event-types
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
