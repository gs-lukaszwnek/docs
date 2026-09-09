---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/changeFeaturedTopics.md
---

# Update the featured topics lists

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/changeFeaturedTopics
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
| `featuredTopics` | array of object | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Event Featured Topics Update updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
