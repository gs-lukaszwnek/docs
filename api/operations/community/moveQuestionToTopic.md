---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveQuestionToTopic.md
---

# Convert a question to a reply

Questions

Move a question with no visible replies to an existing topic. It can be converted to a reply of an article, conversation or another question. This action will permanently delete any trashed replies the question may have.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/moveToTopic
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator moving the question |
| `id` | path | string | Yes | ID of the question to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `topicId` | string | Yes | — |
| `topicType` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Question moved to topic |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
