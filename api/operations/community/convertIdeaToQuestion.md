---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/convertIdeaToQuestion.md
---

# Convert an Idea to a Question

Ideas

A moderator can convert an idea to a question. Note that if an idea has any replies they will be converted as well.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/convertToQuestion
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator converting the idea |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `categoryId` | integer | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 202 | Conversion in progress |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
