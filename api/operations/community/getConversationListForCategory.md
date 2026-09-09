---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getConversationListForCategory.md
---

# List conversations for a category

ConversationsCategories

Fetches a paginated list of conversations in a category. The category must be a public category. The result is sorted by last activity date in descending order. Last activity is the time the conversation was last replied.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories/{id}/conversations
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the category to fetch conversations |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
