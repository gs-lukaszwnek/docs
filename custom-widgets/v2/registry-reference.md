---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/registry-reference.md
description: >-
  Reference for the root of extensions_registry.json — the widgets, scripts,
  stylesheets, and importMaps arrays
---

# Registry Reference

This page documents the root object of `extensions_registry.json`, the file at your repository root that declares the `widgets`, `scripts`, `stylesheets`, and `importMaps` arrays; each entry type has its own reference page linked below.

## Root Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `widgets` | array | No | `[]` | Widget definitions — see [Widget Definition Reference](widget-schema) |
| `scripts` | array | No | `[]` | Script definitions — see [Script Definition Reference](scripts) |
| `stylesheets` | array | No | `[]` | Stylesheet definitions — see [Stylesheet Definition Reference](stylesheets) |
| `importMaps` | array | No | `[]` | Shared-dependency import maps — see [Import Maps](#import-maps) below |

All four arrays are optional. A registry with only widgets publishes no scripts, stylesheets, or import maps; a registry with only scripts publishes no widgets. An empty array is valid and publishes nothing of that type.

```json
{
  "widgets": [ ... ],
  "scripts": [ ... ],
  "stylesheets": [ ... ],
  "importMaps": [ ... ]
}
```

## Minimal Example

The smallest valid registry:

```json
{
  "widgets": [],
  "scripts": [],
  "stylesheets": []
}
```

This publishes nothing but passes validation. Use it to verify your repository connection before adding entries.

## File Location

* The file must be at the **root** of your repository.
* Filename must be `extensions_registry.json`.

:::info Migration from `widget_registry.json`
If your repository uses the old `widget_registry.json` filename, it still works during the migration window. Rename it to `extensions_registry.json` at your earliest convenience — the old name is deprecated and will be removed in a future release.
:::

## Validation

* The root must be a JSON object.
* `widgets`, `scripts`, `stylesheets`, and `importMaps` must be arrays if present.
* Each entry follows its own schema — see the per-type reference pages.

For validation errors, see [Error Codes](error-codes).

## Import Maps

`importMaps` declares shared module dependencies (such as a widget's framework) so the same copy can be reused across widgets on a page, instead of each widget bundling its own copy. Each entry:

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `imports` | object | Yes | — | Maps a bare module specifier (e.g. `"react"`) to an `https://` URL to load it from |
| `scopes` | object | No | `null` | Scoped overrides of `imports` for a specific module subtree, per the browser import map spec |
| `integrity` | object | No | `null` | Subresource-integrity hashes for entries in `imports` |

```json
{
  "importMaps": [
    {
      "imports": {
        "react": "https://esm.sh/react@19.2.0",
        "react-dom": "https://esm.sh/react-dom@19.2.0"
      }
    }
  ]
}
```

Every URL in `imports` must be `https://` — non-HTTPS values are rejected at validation, since this is rendered directly into the page as an import map. Declaring `importMaps` makes the dependency available on the page; it does not by itself change how any widget is built — externalizing a widget's own build to use a shared import map is a separate, not-yet-shipped capability of the Developer Studio CLI.

Entries from all connected repositories are concatenated, in order, into one list — they are never merged or deduplicated across repositories.

## Extension Naming

Widget `type` fields and script or stylesheet `name` fields must be unique across every repository connected to a community. When a community connects multiple GitHub organizations (see [Multiple Organizations](multiple-organizations)), each organization's extensions share the same registry namespace, so a naming collision in one repository can shadow an extension from another.

The recommended convention prefixes each name with an organization identifier:

```
{org_name}_{extension_purpose}
```

| Extension | Field | Example |
|-----------|-------|---------|
| Widget | `type` | `acme_corp_welcome_banner` |
| Script | `name` | `acme_corp_analytics` |
| Stylesheet | `name` | `acme_corp_theme` |

```json
{
  "widgets": [
    {
      "type": "acme_corp_welcome_banner",
      "title": "Welcome Banner"
    }
  ],
  "scripts": [
    {
      "name": "acme_corp_analytics",
      "path": "scripts/analytics/script.js"
    }
  ],
  "stylesheets": [
    {
      "name": "acme_corp_theme",
      "path": "stylesheets/theme/style.css"
    }
  ]
}
```

## Next Steps

* [Widget Definition Reference](widget-schema) — fields for the `widgets` array
* [Script Definition Reference](scripts) — fields for the `scripts` array
* [Stylesheet Definition Reference](stylesheets) — fields for the `stylesheets` array
* [Import Maps](#import-maps) — fields for the `importMaps` array
* [Repository Layout](project-setup) — how to organize the files referenced from the registry
