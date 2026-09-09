---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/editIdeaStatus.md
---

# Edit IdeaStatus

Ideas

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/{id}/editIdeaStatus
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `id` | path | string | Yes | Idea Status Id to be edited |
| `moderatorId` | query | string | Yes | ID of the moderator to edit the IdeaStatus |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | — |
| `backgroundColor` | string | No | — |
| `textColor` | string | No | — |
| `default` | boolean | No | — |
| `visible` | boolean | No | — |
| `type` | string | No | The type assigned to an ideation status and lets the system know how to interpret various idea statuses. |

### Responses

| Status | Description |
|--------|-------------|
| 201 | IdeaStatus created |
| 422 | Validation error |
| 500 | Unexpected error |
