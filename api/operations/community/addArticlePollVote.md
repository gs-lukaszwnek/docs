---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/addArticlePollVote.md
---

# Add a vote to a poll attached to the given article

Articles

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/votePoll
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author voting on the poll |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `selectedOption` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Poll vote has been processed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
