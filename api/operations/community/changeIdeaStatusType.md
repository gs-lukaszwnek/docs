---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/changeIdeaStatusType.md
---

# Change an Idea Status type

Ideas

Optionally set a type for an ideation status. Setting a type will help give meaning to statuses in the analytics dashboards..

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/ideas/ideaStatuses/{id}/changeType
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator changing the Idea Status type |
| `id` | path | string | Yes | ID of the idea status to interact with |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | The type is used in analytics dashboards and lets the system know how to interpret various idea statuses. |

### Responses

| Status | Description |
|--------|-------------|
| 204 | Idea Status type was changed |
| 404 | Item not found |
| 422 | Validation error |
| 500 | Unexpected error |
