---
url: https://developer-portal.gainsight.com/docs/api/operations/user/deleteBadge.md
---

# Delete a badge

Gamification: Badges

Permanently delete a badge identified by ID from community and revoke it from all users

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/api1/gamification/badge/{badgeId}
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `badgeId` | path | integer | Yes | The ID of the badge to be deleted |

### Responses

| Status | Description |
|--------|-------------|
| 204 | The badge has been deleted |
| 404 | Not Found |
| 500 | Unexpected error |
