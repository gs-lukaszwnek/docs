---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/community/subscribeWebhook.md
---

# Subscribe URL to webhook event

Webhooks

## Endpoint

```
POST https://api2-eu-west-1.insided.com/v2/webhooks/{eventName}/subscriptions
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `eventName` | path | string | Yes | The event name to subscribe to. |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `url` | string | Yes | — |
| `username` | string | No | — |
| `secret` | string | No | — |
| `auth_type` | string | No | Authentication type. Use 'aws\_sigv4' for AWS Signature V4. Omit for Basic Auth. |
| `role_arn` | string | No | Customer IAM role ARN. Required when auth\_type is aws\_sigv4. |
| `external_id` | string | No | External ID for STS AssumeRole. Required when auth\_type is aws\_sigv4. |
| `region` | string | No | AWS region of the target endpoint. Required when auth\_type is aws\_sigv4. |
| `service` | string | No | AWS service for signing scope. Optional, defaults to execute-api. |

### Responses

| Status | Description |
|--------|-------------|
| 202 | URL subscribed |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
