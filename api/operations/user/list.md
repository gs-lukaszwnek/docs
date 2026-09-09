---
url: https://developer-portal.gainsight.com/docs/api/operations/user/list.md
---

# Fetch list of badges

Gamification: Badges

## Endpoint

```
GET https://api2-eu-west-1.insided.com/api1/gamification/badges
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `page` | query | integer | No | page number of a paginated list starts from 1 |
| `pageSize` | query | integer | No | the amount of results displayed in a paginated list, defaults to 10 |
| `filters` | query | object | No | filters the list: e.g. \&filters\[manualOnly]=1 |

### Responses

| Status | Description |
|--------|-------------|
| 200 | Returns badges |
| 500 | Unexpected error |
