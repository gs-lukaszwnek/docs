---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/editExternalRegistrationUrl.md
---

# Edit an event external registration url

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/editExternalRegistrationUrl
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the event url |
| `id` | path | string | Yes | ID of the event to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `externalRegistrationUrl` | string | Yes | — |
| `externalRegistrationUrlLabel` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event external registration url updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
