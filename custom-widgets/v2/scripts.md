---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/scripts.md
description: >-
  Reference for script entries in the scripts array of extensions_registry.json
  — fields, placement, HTML attributes, and validation rules
---

# Script Definition Reference

This page documents every field of a script entry in the `scripts` array of `extensions_registry.json`. For the root of the registry file, see [Registry Reference](registry-reference). For the mental model of how scripts run, see [Scripts Overview](scripts-overview).

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Unique identifier for the asset. Must start with a letter or number; can include letters, numbers, `_`, `.`, `-`. |
| `path` | string | Yes | Path to the asset file — either a relative path within the repository or an external URL (`https://...`). |
| `description` | string | No | Brief description of what the asset does. |
| `placement` | string | No | Where the asset is injected on the page: `head`, `bodyStart`, or `bodyEnd`. Defaults to `head`. |
| `attributes` | object | No | HTML attributes to add to the generated tag (e.g., `defer`, `async`, `crossorigin`). See [HTML Attributes](#html-attributes) below. |
| `rules` | array | No | [Condition rules](/custom-widgets/v2/page-targeting) controlling which pages the asset loads on. |

## Placement

| Value | Location | Use case |
|-------|----------|----------|
| `head` (default) | Inside `<head>` | Analytics, early initialization, fonts, and styles that must apply before content renders |
| `bodyStart` | Start of `<body>` | Assets that need the DOM to start loading but should run early |
| `bodyEnd` | End of `<body>` | Non-critical assets that depend on page content being fully loaded |

Scripts must have a `path` ending in `.js`.

## Repository-Hosted Scripts

For scripts stored in your repository, use a relative path. The platform publishes the file automatically.

Place each script in its own directory: `scripts/<name>/script.js`

```json
{
  "scripts": [
    {
      "name": "analytics",
      "path": "scripts/analytics/script.js",
      "description": "Tracks widget impressions and interactions",
      "placement": "head"
    }
  ]
}
```

## External Scripts

If `path` is an external URL, the script is loaded directly from that URL — it is **not** published by the platform.

```json
{
  "scripts": [
    {
      "name": "external-tracker",
      "path": "https://cdn.example.com/tracker.js",
      "description": "Third-party event tracking",
      "placement": "bodyEnd"
    }
  ]
}
```

## HTML Attributes

Use the `attributes` field to add standard HTML attributes to the generated `<script>` tag. This gives you control over script loading behavior and execution timing.

```json
{
  "scripts": [
    {
      "name": "analytics",
      "path": "scripts/analytics/script.js",
      "placement": "head",
      "attributes": {
        "defer": ""
      }
    }
  ]
}
```

This produces: `<script src="..." defer></script>`

The `attributes` field is a plain object where each key is the attribute name and the value is the attribute value. Use an empty string (`""`) for boolean attributes like `defer` and `async`.

### Common Attributes

| Attribute | Value | Effect |
|-----------|-------|--------|
| `defer` | `""` | Script executes after HTML parsing completes — does not block page rendering |
| `async` | `""` | Script downloads in parallel and executes as soon as it's ready |
| `crossorigin` | `"anonymous"` | Enables CORS requests without sending credentials |
| `crossorigin` | `"use-credentials"` | Enables CORS requests with credentials |

### When to Use `defer`

Use `defer` when your script depends on the DOM being fully parsed but should not block the initial page render. This is the most common use case for custom scripts — analytics, tracking, and UI enhancements that run after the page loads.

```json
{
  "scripts": [
    {
      "name": "post-render-init",
      "path": "scripts/init/script.js",
      "placement": "head",
      "attributes": {
        "defer": ""
      }
    }
  ]
}
```

::: tip defer vs placement
`defer` and `placement` serve different purposes. `placement` controls **where** in the HTML the `<script>` tag appears. `defer` controls **when** the script executes — always after parsing, regardless of placement. For most non-blocking scripts, `"placement": "head"` with `"defer": ""` is the recommended approach.
:::

### Multiple Attributes

You can combine multiple attributes on a single script:

```json
{
  "attributes": {
    "defer": "",
    "crossorigin": "anonymous"
  }
}
```

This produces: `<script src="..." defer crossorigin="anonymous"></script>`

## Conditional Loading

Use `rules` to control which pages a script loads on. Without rules, the script loads on every page. See [Page Targeting](page-targeting) for the full rules reference.

```json
{
  "scripts": [
    {
      "name": "homepage-hero",
      "path": "scripts/hero/script.js",
      "rules": [
        { "field": "page", "operator": "eq", "value": "homepage" }
      ]
    },
    {
      "name": "user-analytics",
      "path": "scripts/analytics/script.js",
      "rules": [
        { "field": "authenticated", "operator": "eq", "value": "true" }
      ]
    }
  ]
}
```

## Validation

* `name` must match `^[a-zA-Z0-9][a-zA-Z0-9_.-]*$`
* `name` must be unique within the `scripts` array
* `path` must end in `.js` — either a relative path or a URL starting with `http://` or `https://`
* `placement` must be one of `head`, `bodyStart`, `bodyEnd`
* `rules` must follow the [condition rules schema](page-targeting#rule-structure)

## Next Steps

* [Scripts Overview](scripts-overview) — mental model for how scripts are injected
* [Your First Script](first-script) — hands-on tutorial
* [Stylesheet Definition Reference](stylesheets) — Add global CSS to your community
* [Page Targeting](page-targeting) — Full context field reference for conditional loading
* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
* [Repository Layout](project-setup) — How to organize your repository
* [Analytics script in the template repository](https://github.com/gainsight-hub/widgets-repository-template/blob/main/scripts/analytics/script.js) — Working global script example
