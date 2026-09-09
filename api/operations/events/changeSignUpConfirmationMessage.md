---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/changeSignUpConfirmationMessage.md
---

# Change the confirmation message that gets displayed after a user signs up for the event

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/changeSignUpConfirmationMessage
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event image |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event image updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
