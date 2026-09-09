---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventImage.md
---

# Edit an event image

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/editImage
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
| `image` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event image updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
