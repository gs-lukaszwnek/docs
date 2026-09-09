---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/DeleteProfileField.md
---

# Delete a custom field in user's profile

User

Returns a Json User

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/{id}/profile_field/{field}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | integer | Yes | The id of the user |
| `field` | path | string | Yes | The name of the field that will be deleted |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Not Found |
| 500 | Unexpected error |
