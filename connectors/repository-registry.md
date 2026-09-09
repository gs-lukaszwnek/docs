---
url: https://developer-portal.gainsight.com/docs/connectors/repository-registry.md
description: >-
  The connectors_registry.json schema for defining connectors in a GitHub
  repository, its fields, and how repository-synced connectors are protected
---

# Repository Registry

Define connectors alongside your widgets in a GitHub repository. Instead of creating each connector manually through the admin UI, add a `connectors_registry.json` file to your repository root and connectors are synced automatically on every push.

## Prerequisites

* A connected GitHub organization (see [Connect Your GitHub Account](/custom-widgets/v2/connect-github))

## When to Use This

* You manage widgets in a GitHub repository (see [Repository Layout](/custom-widgets/v2/project-setup)) and want connectors defined in the same place
* You need consistent connector configurations across multiple communities
* You prefer managing connectors in version control

## How it works

1. Add `connectors_registry.json` to your repository root (alongside `extensions_registry.json`)
2. Push to the watched branch
3. The platform validates the file and syncs connectors automatically
4. Synced connectors appear in the admin UI with a "GitHub" badge and cannot be edited there

The repository is the source of truth. On every push:

* New connector definitions are created
* Changed definitions are updated
* Definitions removed from the file are deleted
* Manually created connectors are never affected

## Repository structure

```
your-repo/
├── extensions_registry.json          # Widget definitions
├── connectors_registry.json      # Connector definitions (optional)
└── widgets/
    └── my_widget/
        └── index.html
```

## Schema

The file must be a JSON object with a `connectors` array:

```json
{
  "connectors": [
    {
      "name": "Idea Endorsements",
      "url": "https://api.example.com/ideas/{{ pathParams.idea_id }}/endorsements",
      "method": "GET",
      "headers": [
        { "key": "Accept", "value": "application/json", "overridable": false }
      ],
      "query_parameters": [
        { "key": "limit", "value": "20", "overridable": true }
      ],
      "path_parameters": [
        { "key": "idea_id" }
      ],
      "authentication": {
        "type": "api_key",
        "config": {
          "key": "X-API-Key",
          "value": "{{ get_secret('endorsements_api_key') }}",
          "in": "header"
        }
      },
      "request_body": "",
      "response_body": "",
      "response_content_type": "",
      "permalink": "idea-endorsements"
    }
  ]
}
```

### Connector fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `name` | string | Yes | — | Display name (max 255 characters) |
| `url` | string | Yes | — | Target endpoint URL (max 2048 characters) |
| `method` | string | No | `"GET"` | HTTP method: GET, POST, PUT, DELETE, PATCH, OPTIONS, HEAD |
| `headers` | array | No | `[]` | Request headers — each key max 255, value max 1000 characters (see [Headers & Query Parameters](/connectors/headers-and-query-parameters)) |
| `query_parameters` | array | No | `[]` | URL query parameters — each key max 255, value max 1000 characters |
| `path_parameters` | array | No | `[]` | Path parameter declarations — each entry is `{key, value?, overridable?}`. `key` must match a {{ pathParams.X }} reference in `url`. See [Dynamic URL Paths](/connectors/dynamic-url-paths). |
| `authentication` | object | No | none | Auth config (see [Authentication](/connectors/authentication#authentication-object)) |
| `request_body` | string | No | `""` | Request transformation template (see [Request Transformation](/connectors/payload-template)) |
| `response_body` | string | No | `""` | Response body template (see [Response Transformation](/connectors/response-transformation)) |
| `response_content_type` | string | No | `""` | Content-Type override for the response returned to the widget (max 255 characters) |
| `permalink` | string | No | auto-generated | URL-safe identifier (`lowercase-with-dashes`, max 255 characters) |

### Permalink

The `permalink` is the connector's identity during sync. If you omit it, one is generated from the name (e.g., "Weather API" becomes `weather-api`).

* Renaming a connector with the same permalink updates the existing connector
* Changing the permalink creates a new connector and deletes the old one
* Permalinks must match the pattern `^[a-z0-9]+(?:-[a-z0-9]+)*$` and be at most 255 characters

### Extra fields

You can copy JSON from the admin UI's [Code Mode](/connectors/code-mode) directly into the file. Any extra fields not listed above are silently ignored.

## Secrets

Connector definitions can reference secrets using Jinja2 syntax:

```json
{
  "name": "Salesforce Lookup",
  "url": "https://myorg.my.salesforce.com/services/data/v59.0/sobjects",
  "authentication": {
    "type": "oauth_client_credentials",
    "config": {
      "client_id": "{{ get_secret('salesforce_client_id') }}",
      "client_secret": "{{ get_secret('salesforce_client_secret') }}",
      "token_url": "https://login.salesforce.com/services/oauth2/token"
    }
  }
}
```

Secrets must be created in advance through the admin UI. See [Secrets and Variables](/connectors/secrets/) for details. The platform validates the connector schema, not secret existence.

::: warning
Never commit actual secret values in `connectors_registry.json`. Always use {{ get\_secret('name') }} to reference secrets stored in the platform.
:::

## Protection

Connectors synced from a repository cannot be edited or deleted through the admin UI or API. All changes must come from the repository.

## Multi-community repositories

When multiple communities subscribe to the same repository, each community gets its own independent set of connectors. Connectors are isolated per community just like widgets.

## Error handling

Any problem with the file fails the entire publish — no connectors are created, updated, or deleted. The build status names the connector and every field to fix; hover over the build status icon for details. Fix the reported problems and push again to sync the file as a whole.

Structural problems:

* **Invalid JSON** — Syntax error in the file (missing comma, trailing comma, etc.)
* **Missing required fields** — A connector is missing `name` or `url`
* **Duplicate permalinks** — Two connectors in the same file share a permalink
* **Permalink collision** — A repository connector's permalink conflicts with a manually created connector. Rename or delete the existing connector to resolve.

A problem with a **single connector's field values** — for example a field that exceeds its length limit, a `permalink` that isn't a lowercase-with-dashes identifier, or an invalid authentication config — also fails the entire publish. The build status names that connector and all of its invalid fields at once, so you can fix everything in one pass.

## Removing the file

If you delete `connectors_registry.json` from your repository, all connectors that were synced from that repository are deleted on the next push. Manually created connectors and connectors from other repositories are not affected.

An empty connectors array has the same effect:

```json
{
  "connectors": []
}
```

## Next Steps

* [Connect Your GitHub Account](/custom-widgets/v2/connect-github) — Connect the GitHub repository this registry file lives in
* [Repository Layout](/custom-widgets/v2/project-setup) — How to set up your widget repository
* [Configuration](configuration) — Connector field details
* [Authentication](authentication) — Auth type options
* [Secrets and Variables](secrets/) — Managing credentials
* [Example `connectors_registry.json` in the template repository](https://github.com/gainsight-hub/widgets-repository-template/blob/main/connectors_registry.json) — Working connector definitions alongside widgets
