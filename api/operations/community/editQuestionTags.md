---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editQuestionTags.md
---

# Tag a question with public tags

Questions

An author can edit tags for their questions. Note that this will overwrite existing tags.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/editTags
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `authorId` | query | string | Yes | ID of the author editing the question tags |
| `id` | path | string | Yes | ID of the question to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tags` | array of Tag | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question tags were changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
