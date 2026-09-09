---
url: https://developer-portal.gainsight.com/docs/api/operations/user/register.md
---

# Register a new user

User

Returns a Json User. The profile\_field option in the request body provides a way to add profile fields to the registration. The key refers to an existing profile field id. Note that profile fields can be set as mandatory registration fields.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/api1/user/register
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `data` | object | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 400 | Validation error |
| 404 | Not Found |
| 500 | Unexpected error |
