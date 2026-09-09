---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getPollResult.md
---

# Show poll results

Conversations

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/conversations/{id}/poll
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the conversation to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
