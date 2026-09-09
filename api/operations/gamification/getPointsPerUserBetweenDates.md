---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/gamification/getPointsPerUserBetweenDates.md
---

# Get assigned points

Points

Get points assigned to users in a timeframe

## Endpoint

```
GET https://api2-eu-west-1.insided.com/gamification/points
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `userId[]` | query | array of integer | Yes | Ids of users to retrive points for. |
| `earnedAt[from]` | query | string | No | Starting value for a timeframe. Uses beginning of unix time as default. |
| `earnedAt[to]` | query | string | No | Ending value for a timeframe. Uses current time as a default. |

### Responses

| Status | Description |
|--------|-------------|
| 200 | List of users with total points assigned |
| 422 | Request is invalid |
| 500 | Unexpected error |
