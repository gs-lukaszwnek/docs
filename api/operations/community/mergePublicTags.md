---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/mergePublicTags.md
---

# Merge public tags

Public Tags

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/tags/merge
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | The ID of the moderator merging tags |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | — |
| `ids` | array of string | Yes | — |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Tags merged |
| 422 | Validation error |
| 500 | Unexpected error |
