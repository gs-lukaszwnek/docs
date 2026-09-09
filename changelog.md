---
url: https://developer-portal.gainsight.com/docs/changelog.md
description: Release history of Developer Studio — new features, changes, and fixes
---

# Changelog

Changes and additions to Developer Studio, ordered by date (newest first).

## 2026-08-27 - Connectors: Templated Hosts in URLs

Connector URLs now accept a Variable in the host, not just in the path and query string — either as one label of the host or as the entire base URL. A single connector definition can therefore target a different host per community. The host can also name a fallback with the `default()` filter, so one connector serves a shared host everywhere and only the communities that need their own have to set the Variable; adding `true` as the second argument falls back when the Variable exists but is empty. See [Configuration](/connectors/configuration#templating-the-host) for details.

## 2026-08-05 - Connectors: Longer Response Window for External APIs

Connectors now allow an external API up to 15 seconds to respond, so integrations with slower endpoints complete successfully instead of returning a proxy error. The same window applies to each step of a composite connector and to the token request for OAuth-authenticated connectors. See [How Connectors Work](/connectors/how-connectors-work) for details.

## 2026-07-29 - Connectors: Variables for Reusable Configuration

Connectors now support **Variables** — named, plain-text configuration values such as base URLs, environment IDs, and instance identifiers that you store once and reference across connectors with `get_variable('name')`. Unlike secrets, a Variable can be used directly in a connector's URL, so you can point the same connector at a sandbox or production host without editing it. See [Secrets and Variables](/connectors/secrets/) for details.

## 2026-07-14 - Widget Manifests: Unknown or Misplaced Attributes Are Rejected

Your widget source manifest (`extensions_registry.json`) now reports a clear error identifying the offending attribute path when it contains an unknown or misplaced key — a misspelled or wrongly-nested attribute — when you sync it from your repository or preview a widget, instead of silently dropping it and reporting success. See [Registry Reference](/custom-widgets/v2/registry-reference) for details.

## 2026-06-24 - Connectors: Duplicate Parameter Keys Are Rejected

Connector definitions now require each header, query parameter, and path parameter key to be unique. Duplicate keys are reported as a clear configuration error when you save a connector, sync it from a repository, or run a connection test — instead of silently keeping only the last value. See [Headers & Query Parameters](/connectors/headers-and-query-parameters) for details.

## 2026-06-17 - Connectors: `api_key` Is Now the Canonical Authentication Type

API key authentication now uses `api_key` as its canonical type identifier (matching the `oauth_client_credentials` / `oauth_jwt_bearer` naming convention). The previous spelling `apikey` is **soft-deprecated** — it is permanently retained as a backward-compatible alias and is never rejected. Both spellings are accepted at all write boundaries and execute identically. Existing connectors previously stored with `"apikey"` are transparently migrated to `api_key`; no manual changes are required. See [Authentication](/connectors/authentication) for details.

## 2026-06-15 - Connector Sync from Repositories Is Now All-or-Nothing

Connector sync from a repository's `connectors_registry.json` is now all-or-nothing: if any connector or composite connector has an invalid field, the entire sync is rejected with a clear error naming the connector and every field to fix, and no changes are applied. A repository's connectors always sync as a consistent set, rather than some going live while others are silently left out. See [Repository Registry](/connectors/repository-registry) for details.

## 2026-06-15 - Clearer Connector Build Errors and Documented Field Limits

When a connector in `connectors_registry.json` has an invalid or over-length field, the build error now names the connector and the field to fix instead of showing internal details. Connector field limits — URL, name, header and query values, response content type, and permalink — are now documented. See [Repository Registry](/connectors/repository-registry) for details.

## 2026-06-12 - Connector URLs Support Up to 2048 Characters

Connector URLs now support up to 2048 characters, accommodating templates with multiple path and query parameter placeholders. Repositories whose connector sync was failing due to URL length will succeed on the next push. See [Dynamic URL Path Segments](/connectors/dynamic-url-paths) for details on URL templates.

## 2026-05-27 - Dynamic URL Path Segments in Connectors

Connectors can now use caller-supplied values as path segments in the upstream URL — for example, fetching `https://api.example.com/ideas/42/endorsements` where `42` is supplied by the widget per call. Declare each variable segment as a **path parameter** and reference it from the URL with {{ pathParams.X }}. Values are validated as URL-safe (letters, digits, and `. _ ~ -`; max 200 chars; no path-traversal segments) before any outbound call is made, and the upstream host stays locked to the connector's configured prefix. Callers pass the values via the new `pathParams` field on the SDK's `execute` method. See [Dynamic URL Path Segments](/connectors/dynamic-url-paths) for details.

## 2026-05-14 - Composite Connectors

Chain multiple API calls into a single execution. Define an ordered sequence of steps in `connectors_registry.json` — each step calls an external API, and its response feeds into the next step via Jinja2 templates. The final step's response is returned to the widget. Supports per-step authentication, response transformation, and access to the client's incoming request data. See [Build a Composite Connector](/connectors/composite-connectors) for details.

## 2026-04-29 - Scripts and Stylesheets: `attributes` Field Now Honored

The `attributes` object on script and stylesheet entries in `extensions_registry.json` was being silently dropped during validation. It is now preserved end-to-end and appears in the widget registry response. Repositories already declaring `attributes` will see them take effect on the next registry refresh.

## 2026-04-22 - Build Status: Non-Fatal Advisory Warnings

The build status on GitHub repository sources now includes a `warnings` array alongside `state` and `error`, surfacing non-fatal advisories detected on the latest build. The first advisory reports when a repository still uses the deprecated `widget_registry.json` filename, so you can spot the rename opportunity before the legacy fallback is removed.

## 2026-04-22 - Widget Registry: `settings.shared` Default Is Now `false`

When a widget in `extensions_registry.json` omits `settings.shared`, it now defaults to `false` (previously `true`). Authors who want cross-page sharing must set `settings.shared` to `true` explicitly. See [Widget Definition Reference](/custom-widgets/v2/widget-schema#settings-object) for the full settings reference.

## 2026-04-22 - Widget Registry: Non-Array `configuration.properties` Rejected with Targeted Error

In `extensions_registry.json`, `configuration.properties` must be an array of field definitions. Builds with the common JSON-Schema-style object shape are now rejected with a targeted error showing the correct shape. See [Widget Definition Reference](/custom-widgets/v2/widget-schema#configuration-object) for details.

## 2026-04-22 - Widget Registry: `category` Is Now Required

Every widget in `extensions_registry.json` must declare a `category`. Publishes without one are rejected with a clear error identifying the missing field. Existing widgets that already declare a category are unaffected. See [Widget Definition Reference](/custom-widgets/v2/widget-schema) for the full widget schema.

## 2026-04-21 - Extensions Registry: Canonical Filename Renamed to `extensions_registry.json`

The registry file at the root of your repository is now named `extensions_registry.json`. The old filename `widget_registry.json` is deprecated and still works during a migration window, but will be removed in a future release — rename it at your earliest convenience. No schema changes are required; the file contents remain identical. See [Repository Layout](/custom-widgets/v2/project-setup) for details.

## 2026-04-20 - HTML Attributes for Scripts and Stylesheets

Custom scripts and stylesheets now support an optional `attributes` field for standard HTML attributes like `defer`, `async`, and `crossorigin`, giving widget creators control over loading behavior and execution timing. See [Script Definition Reference](/custom-widgets/v2/scripts#html-attributes) and [Stylesheet Definition Reference](/custom-widgets/v2/stylesheets#html-attributes) for details.

## 2026-04-16 - Removed Legacy Template Customization for Widgets

Widget HTML is no longer processed as a Jinja2 template at render time. Previously, query parameters on the render URL were injected into {{ }} placeholders in widget HTML — this legacy mechanism has been removed. Widgets that used this pattern should migrate to [`sdk.getProps()`](/custom-widgets/v2/configurable-widgets) for all runtime configuration needs.

## 2026-03-30 - Response Transformation for Connectors

Connectors can now reshape upstream API responses before returning them to the widget. Configure a Jinja2 template to extract specific fields, reformat JSON, or normalize error responses — all on the server, without any client-side logic. An optional content-type override lets you change the response format when the template produces a different output type. See [Response Transformation](/connectors/response-transformation) for details.

## 2026-02-25 - Repository-Based Connector Definitions

Connectors can now be defined alongside widgets in a `connectors_registry.json` file at the repository root, validated and synced automatically on every push — ensuring the repository remains the single source of truth. See [Repository Registry](/connectors/repository-registry) for details.

## 2026-02-19 - Manage GitHub Accounts and Disconnect

You can now manage GitHub account connections from one place: connect additional accounts, disconnect an account from your community without uninstalling the app from GitHub, and reconnect later if needed. See [Connect Your GitHub Account](/custom-widgets/v2/connect-github) for details.

## 2026-02-19 - Credential Scanning

Widget code is now scanned for hardcoded credentials — including API keys, private keys, and access tokens — before publishing, with flagged builds blocked to prevent accidental secret exposure. See [Content Security](/custom-widgets/v2/content-security) for details.

## 2026-02-12 - Widget Thumbnails via Relative imageSrc

Widget creators can now use relative paths in imageSrc for widget preview images, keeping thumbnails in the same repository as the widget instead of hosting them externally.

## 2026-02-12 - Content Safety Scanning

Widget code is now automatically scanned for security issues — including crypto mining, data exfiltration, phishing, and obfuscated code — before publishing, with flagged builds rejected alongside a clear error message. See [Content Security](/custom-widgets/v2/content-security) for details.

## 2026-02-11 - Security Headers and Cache Invalidation

Widget responses now include Referrer-Policy and Permissions-Policy headers, strengthening browser-level protections for embedded widgets. Additionally, deleting a widget now automatically removes cached content, ensuring stale content is no longer served after removal.

## 2026-02-10 - Delimiter-Separated Values in Connector Configuration

Connector configuration now accepts delimiter-separated values (comma, pipe, semicolon) in query parameters and headers, such as field selection lists commonly used when calling external APIs.

## 2026-02-04 - Widget Asset Co-location

Widget creators can now store assets (CSS, JS, images) alongside widget code and reference them using standard HTML relative paths, simplifying development and improving load performance.

## 2026-02-04 - Secure Secret References in Connector Configuration

Connector configuration now enforces secret references for credential fields, ensuring sensitive values are always stored securely.
