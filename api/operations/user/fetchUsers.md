---
url: https://developer-portal.gainsight.com/docs/api/operations/user/fetchUsers.md
---

# Fetches all users

User

Fetches all users

## Endpoint

```
GET https://api2-eu-west-1.insided.com/api1/user
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `page` | query | integer | No | page number of a paginated list starts from 1 |
| `pageSize` | query | integer | No | the amount of results displayed in a paginated list, defaults to 25 |
| `_returnIterable` | query | boolean | No | whether to return users as iterable or not. Defaults to false for backwards compatibility |
| `filter[roles.rolename][]` | query | array of string | No | Filter by role names. Use multiple values for filtering by multiple roles. |
| `filter[badges.badgeid][]` | query | array of integer | No | Filter by badge IDs. Use multiple values for filtering by multiple badges. |
| `filter[userid][]` | query | array of integer | No | Filter by user IDs. Use multiple values for filtering by multiple users. |
| `filter[joindate][from]` | query | string | No | Filter users who joined on or after this date (yyyy-mm-dd format). |
| `filter[joindate][to]` | query | string | No | Filter users who joined on or before this date (yyyy-mm-dd format). |
| `filter[lastactivity][from]` | query | string | No | Filter users whose last activity was on or after this date (yyyy-mm-dd format). |
| `filter[lastactivity][to]` | query | string | No | Filter users whose last activity was on or before this date (yyyy-mm-dd format). |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Returns users |
| 404 | Not Found |
| 500 | Unexpected error |
