---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/askQuestion.md
---

# Ask a question

Questions

An author can ask a question in a category, providing the title and content.    A question asked by an author is always open, meaning the original author and other authors can reply to it. Authors can also like (& unlike!) a question and individual replies to the question. Authors can also optionally add public tags when asking a question. Authors can optionally ask a question with a poll. A poll consists of a title and options. Existing authors can vote on the poll attached to the question.  Moderators can optionally ask a question as closed, which means that only moderators can reply to it. Moderators can also optionally ask a question as sticky, which highlights the question at the top of the category on the community.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/ask
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author of the question |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `content` | string | Yes | — |
| `categoryId` | integer | Yes | — |
| `poll` | object | No | — |
| `tags` | array of Tag | No | — |
| `sticky` | boolean | No | Setting this property requires a moderator |
| `closed` | boolean | No | Setting this property requires a moderator |
| `moderatorTags` | array of Tag | No | Setting this property requires a moderator |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Question asked |
| 400 | Malformed input |
| 403 | Category restricted |
| 422 | Validation error |
| 500 | Unexpected error |
