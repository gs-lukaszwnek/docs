---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/bulkUserRemoveRole.md
---

# Bulk remove roles from users

User

Bulk remove roles from users

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/bulk/role
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
