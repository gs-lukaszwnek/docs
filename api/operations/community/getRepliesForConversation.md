---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getRepliesForConversation.md
---

# List replies for a conversation

Conversations

Lists paginated set of replies for a conversation. By default returns visible replies. A valid `moderatorId` value is required to fetch trashed replies.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/conversations/{id}/replies
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |
| `moderatorId` | query | string | No | ID of the moderator fetching the replies |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |
| `sort` | query | string | No | Defines the field to sort the results on. By default the response is sorted by oldest first. oldestFirst : The least recent reply to the most recent reply.mostRecentFirst : Ordered by most recent to least recent. mostLiked : Ordered by replies which have the most likes |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
