---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editQuestionPollTitle.md
---

# Edit the poll title

Questions

A moderator can edit the title of a poll. If a poll option has votes, then it cannot be edited.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/editPollTitle
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
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question poll title was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
