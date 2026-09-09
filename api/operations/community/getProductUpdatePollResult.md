---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getProductUpdatePollResult.md
---

# Show poll results

ProductUpdates

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/poll
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the productUpdate to fetch the poll results for |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Item not found |
| 500 | Unexpected error |
