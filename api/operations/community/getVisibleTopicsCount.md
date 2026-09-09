---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/getVisibleTopicsCount.md
---

# Get visible topics count per category

Categories

Returns the count of visible (non-spam, non-trashed, non-pending, non-draft) topics per category. This includes questions, conversations, ideas, articles, and product updates. Note: This endpoint does not return counts for sections.

## Endpoint

```
GET https://api2-eu-west-1.insided.com/v2/categories/getVisibleTopicsCount
```

### Responses

| Status | Description |
|--------|-------------|
| 200 | Successful response |
| 500 | Unexpected error |
