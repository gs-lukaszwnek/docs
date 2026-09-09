---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/promoteReply.md
---

# Promote an existing reply to a new conversation

Conversations

A moderator can promote a reply to a new conversation. The title and category for the conversation are required in the request body.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{id}/promote
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator promoting the reply |
| `id` | path | string | Yes | Reply ID |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `categoryId` | integer | Yes | — |
| `title` | string | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Reply promoted |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
