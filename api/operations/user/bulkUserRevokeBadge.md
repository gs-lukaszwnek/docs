---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/bulkUserRevokeBadge.md
---

# Bulk revoke badges from users

User

Bulk revoke badges from users

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/bulk/badge
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `data` | object | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 |  |
| 404 | Not Found |
| 500 | Unexpected error |
