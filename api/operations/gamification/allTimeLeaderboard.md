---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/gamification/allTimeLeaderboard.md
---

# All time leaderboard

Leaderboard

Fetches a paginated list of users sorted by ascending order of all time points earned

## Endpoint

```
GET https://api2-eu-west-1.insided.com/gamification/leaderboard
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `excluded[]` | query | array of string | No | List of user roles to exclude. |
| `page` | query | integer | No | Selects the page to retrieve on paginated responses. Used in combination with the `pageSize` parameter to paginate over results |
| `pageSize` | query | integer | No | Limits the number of items returned in a single paginated response. Used in combination with the `page` parameter to paginate over results |

### Responses

| Status | Description |
|--------|-------------|
| 200 | List of users on the leaderboard |
| 500 | Unexpected error |
