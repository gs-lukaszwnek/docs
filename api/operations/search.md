---
url: https://developer-portal.gainsight.com/docs/api/operations/search.md
---

# Index content for federated search

Federated Search

Endpoint to add or update content that will be available for search. Any data passed to this endpoint will be either added (if the url doesn't already exist), updated (if the url is already present). A maximum of 1000 records per batch is allowed, with a total maximum of 25000 records by default (configurable per community). Maximum size of the payload per request is 5M. The source value will be set to 'federated' when not specified.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/search/external-content/index
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `batch` | array of object | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Success response is empty |
| 400 | Validation error |
| 413 | Payload is too big, try sending less data in a batch |
