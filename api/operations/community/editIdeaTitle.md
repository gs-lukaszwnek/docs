---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editIdeaTitle.md
---

# Edit the title of an idea

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/editTitle
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the idea title |
| `id` | path | string | Yes | ID of the idea to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea title was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
