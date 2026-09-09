---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/reportQuestion.md
---

# Report a question

Questions

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/questions/{id}/report
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | ID of the question to interact with |
| `reportedBy` | query | string | Yes | ID of the author reporting the question |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reason` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Question was reported |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
