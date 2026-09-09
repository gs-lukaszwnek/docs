---
url: https://developer-portal.gainsight.com/docs/api/operations/search/delete.md
---

# Delete specific urls from federated search

Federated Search

Endpoint to delete specific items from federated search. Accepts a list of urls that identify content to remove.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/search/external-content/delete
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `urls` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Success response is empty |
| 400 | Validation error |
