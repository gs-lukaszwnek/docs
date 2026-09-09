---
url: https://developer-portal.gainsight.com/docs/api/operations/user/awardBadge.md
---

# Award a badge to an user

User: Badges

Award a badge identified by ID to the user identified by the user ID

## Endpoint

```
PUT https://api2-eu-west-1.insided.com/api1/user/{userId}/badge/{badgeId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `userId` | path | integer | Yes | The ID of the user to award the badge to |
| `badgeId` | path | integer | Yes | The ID of the badge to be awarded |

### Responses

| Status | Description |
|--------|-------------|
| 201 | The badge has been awarded |
| 204 | The badge was already awarded to this user |
| 404 | Not Found |
| 500 | Unexpected error |
