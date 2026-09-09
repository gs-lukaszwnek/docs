---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getQuestionListForCategory.md
---

# List questions for a category

QuestionsCategories

Fetches a paginated list of questions in a category. The category must be a public category. The result is sorted by last activity in descending order. Last activity is the time the question was last replied.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories/{id}/questions
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the category to fetch questions for |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
