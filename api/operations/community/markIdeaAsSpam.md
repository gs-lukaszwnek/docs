---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/markIdeaAsSpam.md
---

# Mark idea as spam

Ideas

This action automatically bans the author of the content by default. Ensure the content clearly violates spam policies before proceeding.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/markAsSpam
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the idea to interact with |
| `moderatorId` | query | string | Yes | ID of the moderator |

### Request Body

`application/json`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `banUser` | boolean | No | Whether to ban the author of the content. Defaults to true if not specified. |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea marked as spam |
| 422 | Validation error |
| 500 | Unexpected error |
