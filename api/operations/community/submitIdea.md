---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/submitIdea.md
---

# Submit an idea

Ideas

Authors can submit an idea, providing the title and content of the idea.  A  submitted idea by an author is always open, meaning the original author and other authors can reply to it. The author's vote is added automatically and they also can unvote their ideas. Authors can also optionally add public tags and product areas when submitting an idea.  Moderators can optionally submit an idea as closed, which means that only moderators can reply to it.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/submit
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of author of the idea |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `content` | string | Yes | — |
| `productAreaIds` | string | No | — |
| `tags` | array of Tag | No | — |
| `categoryId` | integer | No | The ID of the category to submit the idea in. Required when the community has enabled categories for ideas and product updates. |
| `sticky` | boolean | No | Setting this property requires a moderator |
| `closed` | boolean | No | Setting this property requires a moderator |
| `moderatorTags` | array of Tag | No | Setting this property requires a moderator |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Idea submitted |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
