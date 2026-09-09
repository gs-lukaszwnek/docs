---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getQuestionPollResult.md
---

# Show poll results

Questions

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/questions/{id}/poll
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
