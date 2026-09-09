---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getTrashedConversationList.md
---

# List trashed conversations

Conversations

Fetches a paginated list of trashed conversations. The result is sorted by last activity in descending order. Last activity is the time the conversation was last replied.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/conversations/trashed
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator fetching the trashed conversation list |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
