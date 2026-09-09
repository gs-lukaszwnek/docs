---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/gamification/assignPoints.md
---

# Assign points

Points

Assign points to a single user by UserId

## Endpoint

```
POST https://api2-eu-west-1.insided.com/gamification/points/assign
```

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `user` | integer | No | UserId |
| `points` | integer | No | Amount of points to assign |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Points has been assigned |
| 500 | Unexpected error |
