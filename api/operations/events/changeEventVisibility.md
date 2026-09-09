---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/changeEventVisibility.md
---

# Changes visibility for an event, when provided with a user group id, it is only visible in the user group, else it is publicly visible.

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/changeVisibility
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event title |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `userGroupId` | string | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event visibility changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
