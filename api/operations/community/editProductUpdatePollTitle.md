---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdatePollTitle.md
---

# Edit the poll title

ProductUpdates

A moderator can edit the title of a poll. If a poll option has votes, then it cannot be edited.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editPollTitle
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the productUpdate poll title |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate poll title was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
