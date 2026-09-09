---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/toggleQuestionClosed.md
---

# Open/Close a question

Questions

A moderator can open or close a question for replies.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/toggleClosed
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator toggling the closed state of this question |
| `id` | path | string | Yes | ID of the question to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `closed` | boolean | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question closed state was updated |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
