---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventTitle.md
---

# Edit an event title

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/editTitle
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
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event title updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
