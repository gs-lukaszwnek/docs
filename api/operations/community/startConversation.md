---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/startConversation.md
---

# Start a conversation

Conversations

Authors can start a conversation in a category, providing the title and content of the conversation.  A conversation started by an author is always open, meaning the original author and other authors can reply to it. Authors can also like (& unlike!) a conversation and individual replies within the conversation. Authors can also optionally add public tags when starting a conversation. Authors can optionally start a conversation with a poll. A poll consists of a title and options. Existing authors can vote on the poll attached to conversations.  Moderators can optionally start a conversation as closed, which means that only moderators can reply to it. Moderators can also optionally start a conversation as sticky, which highlights the conversation at the top of the category on the community.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/conversations/start
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | The ID of author of the conversation |

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
| 201 | Conversation started |
| 400 | Malformed input |
| 403 | Category restricted |
| 422 | Validation error |
| 500 | Unexpected error |
