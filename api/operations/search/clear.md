---
url: https://developer-portal.gainsight.com/docs/api/operations/search/clear.md
---

# Delete federated search content for specified source

Federated Search

Endpoint to delete federated search content for specified source. The 'source' parameter is required and indicates which external system's content to remove. Valid values include: 'federated', 'zendesk', 'intercom', 'freshdesk', or a custom source identifier provided when the content was originally added.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/search/external-content/clear
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `source` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Success response is empty |
