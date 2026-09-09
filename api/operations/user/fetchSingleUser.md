---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/fetchSingleUser.md
---

# Fetch a single user

User

Fetch a single user by UserId

## Endpoint

```
GET https://api2-eu-west-1.insided.com/api1/user/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | integer | Yes | The id of the user to be retrieved |

### Responses

| Status | Description |
|--------|-------------|
| 200 | User data |
| 404 | Not Found |
| 500 | Unexpected error |
