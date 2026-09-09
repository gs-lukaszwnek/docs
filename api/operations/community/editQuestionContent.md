---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editQuestionContent.md
---

# Edit the opening post content of a question

Questions

An author can change the content of a question for a limited time after asking the question. The default edit time span is 60 minutes.  Moderators can always change the content of a question that was started by any other author.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/editContent
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author or moderator making the edits |
| `id` | path | string | Yes | ID of the question to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Question edited |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
