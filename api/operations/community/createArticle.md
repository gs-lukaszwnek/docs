---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/createArticle.md
---

# Create an article

Articles

A moderator can create an article in a category, providing the title and content.    An article is always created as a draft (which later can be published) and open (authors can reply to it). Moderators can optionally add public labels to provide more context about the type of article. It can also optionally have a featured image with a valid url. The allowed image extensions for featured image are `png`, `jpeg`, `jpg`, `gif`. Moderators can optionally create an article with a poll. A poll consists of a title and options. Existing authors can vote on the poll attached to an article. Authors can like (& unlike!) an article and individual replies to the article.   Moderators can optionally create an article as closed, which means that only moderators can reply to it. Moderators can also optionally create an article sticky, which highlights the article at the top of the category on the community.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/create
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author of the article |
| `moderatorId` | query | string | No | The ID of the moderator who will create the article on behalf of the author (who in this case can be a registered user as well) |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `featuredImage` | string | No | Supported types: 'png', 'jpeg', 'jpg', 'gif' |
| `featuredImageAltText` | string | No | Alternative text description for the featured image |
| `publicLabel` | string | No | — |
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
| 201 | Article created |
| 400 | Malformed input |
| 403 | Category restricted |
| 422 | Validation error |
| 500 | Unexpected error |
