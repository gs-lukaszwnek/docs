---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/getEventList.md
---

# List events

Events

Fetches a paginated list of events sorted by startdate in ascending order

## Endpoint

```
GET https://api2-eu-west-1.insided.com/events/events
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `X-USER-ID` | header | number | No | Non moderator id of the user fetching the data. If `moderatorId` query parameter is provided, it takes precedence. If provided, public and user's group events are returned. |
| `moderatorId` | query | string | No | ID of the moderator requesting the list of events. Takes precedence over `X-USER_ID` header. |
| `class` | query | string | No | Retrieve a certain classification of events. One of 'past', 'published', 'upcoming', 'all', 'public'. `all` requires `moderatorId` to be provided. |
| `filter` | query | string | No | Deprecated. Use `class` instead. If both are provided, `class` takes precendence. |
| `filters` | query | object | No | Filter the list of events |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |
| `order` | query | string | No | Sets the order of the event list. |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
