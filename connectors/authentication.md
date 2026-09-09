---
url: https://developer-portal.gainsight.com/docs/connectors/authentication.md
description: >-
  Connector authentication types — None, API Key, OAuth Client Credentials, JWT,
  and OAuth JWT Bearer — and their configuration fields
---

# Authentication

Each connector has an **Authentication Type** that controls how the platform proves your identity to the external API. Credentials are always injected server-side, so they are never exposed to the browser.

Every authentication type follows the same pattern: when a connector executes, the platform applies authentication to the outgoing HTTP request **before** it leaves the server. Depending on the type, this may be as simple as adding a header, or as complex as exchanging a signed token with an OAuth provider.

## Authentication Types

:::tip Which type should you use?
See [Choose an Authentication Type](choose-authentication) for a quick decision guide.
:::

Each type below is documented with the same structure: a description of what it does, a sequence diagram showing the request flow, the configuration fields, an example of what the external API actually receives, and (where applicable) how tokens are managed.

***

### None

No authentication. The platform sends the request exactly as configured, with no additional credentials.

**Request flow**

```mermaid
sequenceDiagram
    participant W as Widget
    participant P as Platform
    participant A as External API

    W->>P: SDK connector call
    P->>A: Request with configured<br/>headers and query params only
    A-->>P: Response
    P-->>W: Response data
```

**What the API receives**

```http
GET /v1/public/data HTTP/1.1
Host: api.example.com
Content-Type: application/json
```

No authentication headers or credentials are added. Use this for public APIs that require no authentication.

***

### API Key

Injects a static key-value pair into every request. The key can be placed in either a **header** or a **query parameter**, depending on what the external API expects.

**Request flow**

```mermaid
sequenceDiagram
    participant W as Widget
    participant P as Platform
    participant A as External API

    W->>P: SDK connector call
    Note over P: Inject API key into<br/>header or query string
    P->>A: Request with API key<br/>attached to header or URL
    A-->>P: Response
    P-->>W: Response data
```

**Configuration**

| Field | Description |
|-------|-------------|
| **Key** | The name to send, e.g. `X-API-Key` or `key` |
| **Value** | The credential value — use a [Secrets and Variables](secrets/), e.g. `get_secret('service_api_key')` |
| **In** | Where to place it: **Header** or **Query Parameter** |

All authentication fields accept [Jinja2](https://jinja.palletsprojects.com/) expressions. To reference a stored secret, type the full expression into the field, for example: {{ get\_secret('service\_api\_key') }}

**What the API receives**

When **In** is set to **Header**:

```http
GET /v1/weather?q=Warsaw HTTP/1.1
Host: api.weatherapi.com
X-API-Key: sk-abc123...
```

When **In** is set to **Query Parameter**:

```http
GET /v1/weather?q=Warsaw&key=sk-abc123... HTTP/1.1
Host: api.weatherapi.com
```

::: tip
Use [Secrets and Variables](secrets/) for the **Value** field instead of hardcoding tokens. This keeps credentials encrypted and out of connector configuration.
:::

***

### OAuth Client Credentials

The platform requests an access token from your OAuth server using your **Client ID** and **Client Secret**, caches it, and injects it as a `Bearer` token on every connector request. This is the standard [OAuth 2.0 Client Credentials](https://datatracker.ietf.org/doc/html/rfc6749#section-4.4) flow, designed for server-to-server communication where no user interaction is needed.

**Request flow**

```mermaid
sequenceDiagram
    participant W as Widget
    participant P as Platform
    participant C as Token Cache
    participant T as Token Endpoint
    participant A as External API

    W->>P: SDK connector call
    P->>C: Look up cached token
    alt Token is cached and valid
        C-->>P: Cached Bearer token
    else Token missing or expired
        C-->>P: Cache miss
        P->>T: POST token_url<br/>grant_type=client_credentials
        T-->>P: access_token + expires_in
        P->>C: Cache token until expiry
    end
    P->>A: GET /api/resource<br/>Authorization: Bearer [token]
    A-->>P: Response
    P-->>W: Response data
```

**Configuration**

| Field | Description |
|-------|-------------|
| **Client ID** | The OAuth client identifier — use a [Secrets and Variables](secrets/), e.g. `get_secret('salesforce_client_id')` |
| **Client Secret** | The OAuth client secret — use a [Secrets and Variables](secrets/), e.g. `get_secret('salesforce_client_secret')` |
| **Token URL** | The OAuth token endpoint, e.g. `https://login.salesforce.com/services/oauth2/token` |
| **Scope** | *(optional)* The OAuth scope to request, e.g. `api refresh_token` |

**What the API receives**

When the token is missing or expired, the platform first exchanges credentials at your Token URL:

```http
POST /services/oauth2/token HTTP/1.1
Host: login.salesforce.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=3MVG9...&client_secret=E8B1...
```

The token endpoint responds with an access token:

```json
{
  "access_token": "00D5f000000XXXXX!AQcAQH...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

The platform then uses this token on every connector request to your external API:

```http
GET /services/data/v59.0/sobjects/Account HTTP/1.1
Host: mycompany.salesforce.com
Authorization: Bearer 00D5f000000XXXXX!AQcAQH...
```

Subsequent requests reuse the cached token — no token exchange happens until it expires.

**Token lifecycle**

The platform caches the token for the duration specified by your OAuth server's `expires_in` response field. On every connector request the platform checks the cache: if the token is missing or expired, a new token is automatically fetched before the request proceeds.

::: warning Service-account responses are not user-scoped
OAuth Client Credentials authenticate as a service account, not as the logged-in user. The external API applies no per-user permission checks, so the response may include records the user is not entitled to see. Filter those records out in the connector definition before they reach the browser. See [Filtering Sensitive Data](filtering-sensitive-data).
:::

***

### JWT

Signs a JSON Web Token using your private key and injects it directly as a `Bearer` token on every request. Unlike OAuth flows, there is no token exchange with an external server -- the signed JWT **is** the credential.

**Request flow**

```mermaid
sequenceDiagram
    participant W as Widget
    participant P as Platform
    participant A as External API

    W->>P: SDK connector call
    Note over P: Render claims via Jinja2<br/>Sign with private key
    P->>A: GET /api/resource<br/>Authorization: Bearer [signed-jwt]
    A-->>P: Response
    P-->>W: Response data
```

**Configuration**

| Field | Description |
|-------|-------------|
| **Private Key** | The signing key or HMAC secret — use a [Secrets and Variables](secrets/), e.g. `get_secret('signing_private_key')` |
| **Algorithm** | *(optional)* The signing algorithm. Defaults to `RS256`. Supported: `HS256`, `HS384`, `HS512`, `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384`, `PS512`, `EdDSA` |
| **Claims** | A JSON object containing the JWT payload. All values support Jinja2 templates |
| **JWT Headers** | *(optional)* Custom JWT header fields, such as a key identifier (`kid`) |

**Claims example**

```json
{
  "iss": "my-service",
  "sub": "{{ user.email }}",
  "aud": "https://api.example.com",
  "exp": "{{ now(3600) }}",
  "iat": "{{ now() }}"
}
```

Timestamp claims (`exp`, `iat`, `nbf`) are automatically cast to integers when their rendered values are numeric strings. See [Template Variables](template-variables) for all available context variables and functions.

**What the API receives**

The platform renders your claims with current values, producing a payload like:

```json
{
  "iss": "my-service",
  "sub": "john@example.com",
  "aud": "https://api.example.com",
  "exp": 1743007200,
  "iat": 1743003600
}
```

This payload is signed with your private key into a compact JWT (`header.payload.signature`), then sent directly as the Bearer token:

```http
GET /v1/protected/resource HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6Im15LWtleS1pZCJ9.eyJpc3MiOiJteS1zZXJ2aWNlIiwic3ViIjoiam9obkBleGFtcGxlLmNvbSIsImF1ZCI6Imh0dHBzOi8vYXBpLmV4YW1wbGUuY29tIiwiZXhwIjoxNzQzMDA3MjAwLCJpYXQiOjE3NDMwMDM2MDB9.signature
```

There is no token exchange — the signed JWT **is** the credential. The external API verifies the signature using your corresponding public key.

**Token lifecycle**

A fresh JWT is signed on **every connector request**. There is no caching -- each request gets a newly generated token with current claim values. This means dynamic claims like `exp` and `iat` always reflect the current time.

***

### OAuth JWT Bearer

Combines JWT signing with an OAuth token exchange. The platform signs a JWT assertion, exchanges it for an access token at your **Token URL**, caches the token, and injects it as `Authorization: Bearer <token>` for each connector run.

This is common with services like Salesforce that use the [JWT Bearer flow (RFC 7523)](https://datatracker.ietf.org/doc/html/rfc7523).

**Request flow**

```mermaid
sequenceDiagram
    participant W as Widget
    participant P as Platform
    participant C as Token Cache
    participant T as Token Endpoint
    participant A as External API

    W->>P: SDK connector call
    P->>C: Look up cached token
    alt Token is cached and valid
        C-->>P: Cached Bearer token
    else Token missing or expired
        C-->>P: Cache miss
        Note over P: Build JWT assertion<br/>iss=client_id, sub=user<br/>aud=endpoint, exp=now+180s
        Note over P: Sign assertion with private key
        P->>T: POST token_url<br/>grant_type=jwt-bearer<br/>assertion=[signed-jwt]
        T-->>P: access_token
        P->>C: Cache token until expiry
    end
    P->>A: GET /api/resource<br/>Authorization: Bearer [token]
    A-->>P: Response
    P-->>W: Response data
```

**Configuration**

| Field | Description |
|-------|-------------|
| **Client ID** | The OAuth client identifier (used as the `iss` claim) — use a [Secrets and Variables](secrets/), e.g. `get_secret('salesforce_client_id')` |
| **Private Key** | The key used to sign the JWT assertion — use a [Secrets and Variables](secrets/), e.g. `get_secret('salesforce_private_key')` |
| **Token URL** | The OAuth token endpoint, e.g. `https://login.salesforce.com/services/oauth2/token` |
| **Subject** | The user or service account on whose behalf the token is requested, e.g. `admin@example.com` |
| **Audience** | The intended recipient of the assertion, e.g. `https://login.salesforce.com` |
| **Token TTL** | *(optional)* How long to cache the access token, in seconds. Defaults to `3600` |
| **Algorithm** | *(optional)* The JWT signing algorithm. Defaults to `RS256`. Same options as [JWT](#jwt) above |
| **Additional Claims** | *(optional)* Extra claims merged into the JWT assertion payload, e.g. `{ "scope": "api refresh_token" }` |
| **JWT Headers** | *(optional)* Custom JWT header fields, such as a key identifier |

**What the API receives**

When the token is missing or expired, the platform first builds a short-lived JWT assertion:

```json
{
  "iss": "3MVG9...",
  "sub": "admin@example.com",
  "aud": "https://login.salesforce.com",
  "exp": 1743003780,
  "iat": 1743003600
}
```

This assertion is signed with your private key and sent to the Token URL:

```http
POST /services/oauth2/token HTTP/1.1
Host: login.salesforce.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer&assertion=eyJhbGciOiJSUzI1NiJ9.eyJpc3MiOiIzTVZHOS4uLiIsInN1YiI6ImFkbWluQGV4YW1wbGUuY29tIn0.signature
```

The token endpoint responds with a standard OAuth access token:

```json
{
  "access_token": "00D5f000000XXXXX!AQcAQH...",
  "token_type": "Bearer"
}
```

The platform then uses this token on every connector request to your external API:

```http
GET /services/data/v59.0/sobjects/Account HTTP/1.1
Host: mycompany.salesforce.com
Authorization: Bearer 00D5f000000XXXXX!AQcAQH...
```

Note that the JWT assertion is only used during the token exchange — it is never sent to the external API. Subsequent requests reuse the cached access token until it expires.

**Token lifecycle**

The platform caches the access token for the duration specified by **Token TTL**. When the cached token expires, the platform signs a new JWT assertion and exchanges it for a fresh access token automatically.

***

## Authentication object

When configuring connectors programmatically (e.g. via API or code), use these exact JSON structures for each authentication type. All fields support [Jinja2](https://jinja.palletsprojects.com/) templating — use {{ get\_secret('name') }} for sensitive values.

### None type

```json
{
  "type": "none"
}
```

No additional config required.

### API Key type

```json
{
  "type": "api_key",
  "config": {
    "key": "X-API-Key",
    "value": "{{ get_secret('api_key') }}",
    "in": "header"
  }
}
```

**Fields:**

* `key` (string): The header or query parameter name, e.g. `X-API-Key` or `api_key`
* `value` (string): The credential value; use {{ get\_secret('name') }} to reference a stored secret
* `in` (string): Either `"header"` or `"query"`

**Note:** The canonical type is `api_key`. The `apikey` value is also accepted as a deprecated alias and normalized to `api_key` — both validate and resolve identically.

### OAuth Client Credentials type

```json
{
  "type": "oauth_client_credentials",
  "config": {
    "client_id": "{{ get_secret('salesforce_client_id') }}",
    "client_secret": "{{ get_secret('salesforce_client_secret') }}",
    "token_url": "https://login.salesforce.com/services/oauth2/token",
    "scope": "api refresh_token"
  }
}
```

**Fields:**

* `client_id` (string): OAuth client identifier; use a secret
* `client_secret` (string): OAuth client secret; use a secret
* `token_url` (string): The OAuth token endpoint URL
* `scope` (string, optional): OAuth scope(s) to request

### JWT type

```json
{
  "type": "jwt",
  "config": {
    "private_key": "{{ get_secret('signing_private_key') }}",
    "algorithm": "RS256",
    "claims": {
      "iss": "my-service",
      "sub": "{{ user.email }}",
      "aud": "https://api.example.com",
      "exp": "{{ now(3600) }}"
    },
    "jwt_headers": {
      "kid": "my-key-id"
    }
  }
}
```

**Fields:**

* `private_key` (string): The signing key or HMAC secret; use a secret
* `algorithm` (string, optional): Signing algorithm; defaults to `RS256`. Supported: `HS256`, `HS384`, `HS512`, `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384`, `PS512`, `EdDSA`
* `claims` (object): JWT payload as a JSON object; all values support Jinja2 templates
* `jwt_headers` (object, optional): Custom JWT header fields

### OAuth JWT Bearer type

```json
{
  "type": "oauth_jwt_bearer",
  "config": {
    "client_id": "{{ get_secret('salesforce_client_id') }}",
    "private_key": "{{ get_secret('salesforce_private_key') }}",
    "token_url": "https://login.salesforce.com/services/oauth2/token",
    "subject": "admin@example.com",
    "audience": "https://login.salesforce.com",
    "token_ttl": 3600,
    "algorithm": "RS256",
    "additional_claims": {
      "scope": "api refresh_token"
    },
    "jwt_headers": {
      "kid": "my-key-id"
    }
  }
}
```

**Fields:**

* `client_id` (string): OAuth client identifier; used as the JWT `iss` claim; use a secret
* `private_key` (string): Private key to sign the JWT assertion; use a secret
* `token_url` (string): The OAuth token endpoint URL
* `subject` (string): The user or service account on whose behalf the token is requested
* `audience` (string): The intended recipient of the JWT assertion
* `token_ttl` (integer, optional): Seconds to cache the access token; defaults to `3600`
* `algorithm` (string, optional): JWT signing algorithm; defaults to `RS256`. Same options as JWT above
* `additional_claims` (object, optional): Extra claims merged into the JWT assertion
* `jwt_headers` (object, optional): Custom JWT header fields

## Next Steps

* [Secrets and Variables](secrets/) -- Store credentials with `get_secret()` instead of hardcoding values
* [Headers & Query Parameters](headers-and-query-parameters) -- Add static values alongside authentication
* [Testing & Debugging](testing-and-debugging) -- Verify authentication is working

AUTH FLOW DETAILS:

* API Key Header: sets request.headers\[key] = value
* API Key Query: appends key=value to URL query parameters
* OAuth Client Credentials: POSTs grant\_type=client\_credentials to token\_url, caches access\_token for expires\_in-60 seconds
* JWT: signs claims with private key on every request, injects as Bearer token, no caching
* OAuth JWT Bearer: signs assertion (iss=client\_id, sub=subject, aud=audience, exp=now+180s), POSTs grant\_type=urn:ietf:params:oauth:grant-type:jwt-bearer to token\_url, caches access\_token for token\_ttl-60 seconds
