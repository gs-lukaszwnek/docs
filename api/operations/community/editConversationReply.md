---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editConversationReply.md
---

# Edit a reply

Conversations

An author can change the content of the reply for a limited time. Default edit time span is 60 minutes. Moderators can always change the content of the reply by any other author.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/replies/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator making the edits |
| `id` | path | string | Yes | Reply ID |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Reply edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
