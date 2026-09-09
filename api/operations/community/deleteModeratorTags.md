---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/deleteModeratorTags.md
---

# Delete moderator tags

Moderator Tags

Delete moderator tags by ID. More than one ID can be passed in the request body

## Endpoint

```
DELETE https://api2-eu-west-1.insided.com/v2/moderatorTags/delete
```

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator deleting moderator tags |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `moderatorTagIds` | array of string | Yes | Moderator tag IDs to delete. Maximum of 1000 IDs can be provided |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Moderator tags deleted |
| 400 | Validation error |
