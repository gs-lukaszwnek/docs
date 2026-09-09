---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editProductUpdatePublicLabel.md
---

# Edit the public label of a productUpdate

ProductUpdates

A moderator can edit the public label for a productUpdate. Note that this will overwrite existing public label.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/productUpdates/{id}/editPublicLabel
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator editing the public label |
| `id` | path | string | Yes | ID of the product update to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `publicLabel` | string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | ProductUpdate public label was changed |
| 404 | Item not found |
| 422 | Validation error |
