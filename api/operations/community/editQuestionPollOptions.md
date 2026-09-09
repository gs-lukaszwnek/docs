---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editQuestionPollOptions.md
---

# Edit the poll options

Questions

A moderator can edit the options of a poll. If a poll option has votes, then it cannot be edited.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/editPollOptions
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the question poll title |
| `id` | path | string | Yes | ID of the question to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `options` | array of EditQuestionPollOptionsRequest | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question poll options changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
