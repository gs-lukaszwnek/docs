---
url: https://developer-portal.gainsight.com/docs/api/api-authentication.md
description: >-
  How to authenticate requests to the Gainsight CC REST API using OAuth 2.0
  Client Credentials
---

# Authentication

To access the Customer Communities API, you need an access token. Use the OAuth 2.0 flow described below to obtain one and authenticate your requests.

## OAuth 2.0

The Customer Communities API uses the **OAuth 2.0 Client Credentials Grant** flow for machine-to-machine authentication. This flow grants the ability to act on behalf of any user on the platform.

## Obtaining an access token

You'll need your `client_id` and `client_secret`. To create or revoke credentials, log in to your Control environment and navigate to **Integrations > API**.

Request a token by making a `POST` request to the token endpoint:

```bash
curl -X POST https://api2-eu-west-1.insided.com/oauth2/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials' \
  -d 'client_id=<client_id>' \
  -d 'client_secret=<client_secret>' \
  -d 'scope=read'
```

If the credentials are valid, you'll receive a `200` response with the following body:

```json
{
  "token_type": "bearer",
  "access_token": "<access_token>",
  "expires_in": 7200
}
```

:::tip
Only request the scopes your use case requires — avoid requesting broad permissions unnecessarily.
:::

## Scopes

Scopes are space-delimited (URL-encoded). To see all available scopes, refer to the Authentication section of your Control environment. Each endpoint in this reference lists the scope it requires.

## Using the access token

Add the token as an `Authorization` header on every request:

```
Authorization: Bearer <access_token>
```

Reuse the `access_token` for all subsequent requests until it expires. Once expired, obtain a new one by repeating the token request above.
