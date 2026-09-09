---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/gamification/userPosition.md
---

# User's position on the leaderboard.

Leaderboard

Returns leaderboard user with their position on all time or weekly leaderboards.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/gamification/leaderboard/user/{id}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `excluded[]` | query | array of string | No | List of user roles to exclude. |
| `period` | query | string | No | Used to rank using this week leaderboard. Allowed values: weekly, all\_time. All time is the default. |

### Responses

| Status | Description |
|--------|-------------|
| 200 | — |
| 404 | — |
| 500 | Unexpected error |
