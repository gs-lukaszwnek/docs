---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getCategory.md
---

# Find a category by ID

Categories

Finds a category by ID

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the category |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 422 | Validation error |
| 500 | Unexpected error |
