---
url: https://developer-portal.gainsight.com/docs/api/operations/user/erase.md
---

# Deletes an existing user

User

Deletes an existing user and anonymizes content created by the user

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/{id}/erase
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | integer | Yes | The user id to delete. |

### Responses

| Status | Description |
|--------|-------------|
| 202 | User erased |
| 404 | Not Found |
| 500 | Unexpected error |
