---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addArticlePoll.md
---

# Add a poll to an article

Articles

A moderator can add a poll to an article that does not already contain one. Only allowed for draft articles.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/addPoll
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator adding the poll |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `options` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Poll was added to the article |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
