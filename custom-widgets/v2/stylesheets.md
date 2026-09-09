---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/stylesheets.md
description: >-
  Reference for stylesheet entries in the stylesheets array of
  extensions_registry.json — fields, placement, HTML attributes, and validation
  rules
---

# Stylesheet Definition Reference

This page documents every field of a stylesheet entry in the `stylesheets` array of `extensions_registry.json`. For the root of the registry file, see [Registry Reference](registry-reference). For the mental model of how stylesheets load, see [Stylesheets Overview](stylesheets-overview).

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

Stylesheets must have a `path` ending in `.css`.

## Repository-Hosted Stylesheets

For stylesheets stored in your repository, use a relative path. The platform publishes the file automatically.

Place each stylesheet in its own directory: `stylesheets/<name>/style.css`

```json
{
  "stylesheets": [
    {
      "name": "theme",
      "path": "stylesheets/theme/style.css",
      "description": "Shared design tokens and component styles"
    }
  ]
}
```

## External Stylesheets

If `path` is an external URL, the stylesheet is loaded directly from that URL — it is **not** published by the platform.

```json
{
  "stylesheets": [
    {
      "name": "external-fonts",
      "path": "https://fonts.example.com/inter.css",
      "description": "Custom font family"
    }
  ]
}
```

## HTML Attributes

Use the `attributes` field to add standard HTML attributes to the generated `<link>` tag. This gives you control over how stylesheets are loaded and applied.

```json
{
  "stylesheets": [
    {
      "name": "print-styles",
      "path": "stylesheets/print/style.css",
      "attributes": {
        "media": "print"
      }
    }
  ]
}
```

This produces: `<link rel="stylesheet" href="..." media="print">`

The `attributes` field is a plain object where each key is the attribute name and the value is the attribute value.

### Common Attributes

| Attribute | Value | Effect |
|-----------|-------|--------|
| `media` | `"print"` | Stylesheet applies only when printing |
| `media` | `"(max-width: 768px)"` | Stylesheet applies only for viewports up to 768px |
| `crossorigin` | `"anonymous"` | Enables CORS requests without sending credentials |

### Multiple Attributes

You can combine multiple attributes on a single stylesheet:

```json
{
  "attributes": {
    "media": "(prefers-color-scheme: dark)",
    "crossorigin": "anonymous"
  }
}
```

This produces: `<link rel="stylesheet" href="..." media="(prefers-color-scheme: dark)" crossorigin="anonymous">`

## Conditional Loading

Use `rules` to control which pages a stylesheet loads on. Without rules, the stylesheet loads on every page. See [Page Targeting](page-targeting) for the full rules reference.

```json
{
  "stylesheets": [
    {
      "name": "kb-nl-styles",
      "path": "stylesheets/kb-nl/style.css",
      "rules": [
        { "field": "page", "operator": "eq", "value": "knowledgeBase" },
        { "field": "locale", "operator": "eq", "value": "nl-NL" }
      ]
    },
    {
      "name": "category-styles",
      "path": "stylesheets/categories/style.css",
      "rules": [
        { "field": "pageType", "operator": "eq", "value": "category" }
      ]
    }
  ]
}
```

## Validation

* `name` must match `^[a-zA-Z0-9][a-zA-Z0-9_.-]*$`
* `name` must be unique within the `stylesheets` array
* `path` must end in `.css` — either a relative path or a URL starting with `http://` or `https://`
* `placement` must be one of `head`, `bodyStart`, `bodyEnd`
* `rules` must follow the [condition rules schema](page-targeting#rule-structure)

## Next Steps

* [Stylesheets Overview](stylesheets-overview) — mental model for how stylesheets are injected
* [Your First Stylesheet](first-stylesheet) — hands-on tutorial
* [Script Definition Reference](scripts) — Add global JavaScript to your community
* [Page Targeting](page-targeting) — Full context field reference for conditional loading
* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
* [Repository Layout](project-setup) — How to organize your repository
* [Theme stylesheet in the template repository](https://github.com/gainsight-hub/widgets-repository-template/blob/main/stylesheets/theme/style.css) — Working example with CSS custom properties and design tokens
