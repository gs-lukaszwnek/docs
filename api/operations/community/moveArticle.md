---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/moveArticle.md
---

# Move an article to a different category

Articles

A moderator can move an article to a different category. The moderator must have access to the both the category in which the article currently is as well as the category to which the article is to be moved.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/articles/{id}/move
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator moving the article |
| `id` | path | string | Yes | ID of the article to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `categoryId` | integer | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Article was moved |
| 403 | Category restricted |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
