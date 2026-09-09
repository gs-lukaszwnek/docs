---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/choose-authentication.md
description: >-
  Choose the right authentication type for your connector based on what your
  external API requires
---

# Choose an Authentication Type

Not sure which authentication type to use for your connector? Match your external API's requirements to find the right option.

## Decision Guide

| Your API requires... | Use |
|---------------------|-----|
| No authentication | [**None**](authentication#none) |
| A static API key or token | [**API Key**](authentication#api-key) |
| OAuth 2.0 client credentials (client ID + secret) | [**OAuth Client Credentials**](authentication#oauth-client-credentials) |
| A self-signed JWT as the bearer token | [**JWT**](authentication#jwt) |
| A JWT assertion exchanged for an OAuth access token (e.g. Salesforce) | [**OAuth JWT Bearer**](authentication#oauth-jwt-bearer) |

## Start Simple

If you are unsure, start with **API Key** — it covers most third-party APIs. Move to OAuth or JWT when your API provider requires token-based flows.

## Common Scenarios

| Scenario | Recommended type |
|----------|-----------------|
| Weather API or other public API authenticated with a key | API Key |
| Salesforce integration using client credentials | OAuth Client Credentials or OAuth JWT Bearer |
| Internal service that validates signed JWTs directly | JWT |
| Public API with no authentication required | None |

## Next Steps

* [Authentication](authentication) — Configuration fields and request flow for each type
* [Secrets and Variables](secrets/) — Store credentials with `get_secret()` instead of hardcoding values
* [Testing & Debugging](testing-and-debugging) — Verify authentication is working
