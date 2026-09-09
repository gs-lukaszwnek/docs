---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/UpdateProfileField.md
---

# Update a custom field in user's profile

User

Returns a Json User

## Endpoint

```
PUT https://api2-eu-west-1.insided.com/api1/user/{id}/profile_field/{field}/{value}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | integer | Yes | The id of the user |
| `field` | path | string | Yes | The name of the field to be updated |
| `value` | path | string | Yes | The new value (url encoded) |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Not Found |
| 500 | Unexpected error |
