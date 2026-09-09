---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editIdeaContent.md
---

# Edit the opening post content of an idea

Ideas

An author can change the content of an idea for a limited time after submiting the idea. The default edit time span is 60 minutes.  Moderators can always change the content of an idea that was submitted by any other author.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator making the edits |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Idea edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
