---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/user/revoke_badge.md
---

# Revoke badge from user

User: Badges

Revoke badge from user

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/user/{userId}/badge/{badgeId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `userId` | path | integer | Yes | The id of the user to revoke badge from. |
| `badgeId` | path | integer | Yes | The id of the badge to be revoked. |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Badge has been revoked |
| 400 | User with given id doesn't have badge with given id |
| 404 | User not found or Badge not found |
