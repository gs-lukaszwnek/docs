---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editArticlePollOptions.md
---

# Edit the poll options

Articles

A moderator can edit the options of a poll. If a poll option has votes, then it cannot be edited.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/editPollOptions
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the article poll options |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `options` | array of EditArticlePollOptionsRequest | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article poll options was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
