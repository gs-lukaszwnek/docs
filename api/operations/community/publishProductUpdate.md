---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/publishProductUpdate.md
---

# Publish a productUpdate

ProductUpdates

A valid `moderatorId` value is required to publish a productUpdate.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/publish
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the product update to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator publishing the productUpdate |

### Responses

| Status | Description |
|--------|-------------|
| 201 | ProductUpdate published |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
