---
url: https://developer-portal.gainsight.com/docs/api/operations/user/revokeRole.md
---

# Revoke a custom role from a user

User

Revokes a custom role from an existing user

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/{userId}/role/{roleName}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `userId` | path | string | Yes | UserId from which the role will be revoked |
| `roleName` | path | string | Yes | Name of the role to be revoked |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Success response with no content |
| 404 | Not Found |
| 422 | Validation error |
| 500 | Unexpected error |
