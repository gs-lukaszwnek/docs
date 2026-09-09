---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/signupEvent.md
---

# Sign up to an event

Events

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/{id}/signup
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author signing up to the event |
| `id` | path | string | Yes | ID of the event to interact with |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Signed up to event |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
