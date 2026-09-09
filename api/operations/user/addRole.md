---
url: https://developer-portal.gainsight.com/docs/api/operations/user/addRole.md
---

# Add a role to a user

User

Adds a role to an existing user

## Endpoint

```
POST https://api2-eu-west-1.insided.com/api1/user/{id}/role
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | UserId to which role should be added |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `data` | array of string | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Role has been added to the user. Text containing ‘Done' |
| 404 | Not Found |
| 500 | Unexpected error |
