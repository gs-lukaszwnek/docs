---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/createProductUpdate.md
---

# Create a productUpdate

ProductUpdates

A moderator can create a productUpdate, providing the title and content.    An productUpdate is always created as a draft (which later can be published) and open (authors can reply to it). Moderators can optionally add public labels to provide more context about the type of productUpdate. It can also optionally have a featured image with a valid url. The allowed image extensions for featured image are `png`, `jpeg`, `jpg`, `gif`. Moderators can optionally create a productUpdate with a poll. A poll consists of a title and options. Existing authors can vote on the poll attached to a productUpdate. Authors can like (& unlike!) a productUpdate and individual replies to the productUpdate.   Moderators can optionally create a productUpdate as closed, which means that only moderators can reply to it.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/create
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of the author of the productUpdate |
| `moderatorId` | query | string | No | The ID of the moderator who will create the product update on behalf of the author (who in this case can be a registered user as well) |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `featuredImage` | string | No | Supported types: 'png', 'jpeg', 'jpg', 'gif' |
| `featuredImageAltText` | string | No | Alternative text description for the featured image |
| `publicLabel` | string | No | — |
| `content` | string | Yes | — |
| `poll` | object | No | — |
| `tags` | array of Tag | No | — |
| `sticky` | boolean | No | Setting this property requires a moderator |
| `closed` | boolean | No | Setting this property requires a moderator |
| `moderatorTags` | array of Tag | No | Setting this property requires a moderator |
| `createdAt` | integer | No | creation date in Unix timestamp format |
| `productAreas` | array of string | No | A comma-separated list of product area ids the Update refers to |
| `categoryId` | integer | No | The ID of the category to create the product update in. Required when the community has enabled categories for ideas and product updates. |

### Responses

| Status | Description |
|--------|-------------|
| 201 | ProductUpdate created |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
