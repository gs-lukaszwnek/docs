---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editEventContent.md
---

# Edit an event content

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event content |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event content updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
