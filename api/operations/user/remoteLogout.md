---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/remoteLogout.md
---

# Find User by field / value and flag it for logout

User

Generates a Remote logout for a User

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/remotelogout/{field}/{value}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `field` | path | string | Yes | The field to look for |
| `value` | path | string | Yes | The value to look for |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 404 | Not Found |
| 500 | Unexpected error |
