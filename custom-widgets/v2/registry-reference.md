---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/registry-reference.md
description: >-
  Reference for the root of extensions_registry.json — the widgets, scripts, and
  stylesheets arrays
---

# Registry Reference

This page documents the root object of `extensions_registry.json`, the file at your repository root that declares the `widgets`, `scripts`, and `stylesheets` arrays; each entry type has its own reference page linked below.

## Root Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `widgets` | array | No | `[]` | Widget definitions — see [Widget Definition Reference](widget-schema) |
| `scripts` | array | No | `[]` | Script definitions — see [Script Definition Reference](scripts) |
| `stylesheets` | array | No | `[]` | Stylesheet definitions — see [Stylesheet Definition Reference](stylesheets) |

All three arrays are optional. A registry with only widgets publishes no scripts or stylesheets; a registry with only scripts publishes no widgets. An empty array is valid and publishes nothing of that type.

```json
{
  "widgets": [ ... ],
  "scripts": [ ... ],
  "stylesheets": [ ... ]
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
* `widgets`, `scripts`, and `stylesheets` must be arrays if present.
* Each entry follows its own schema — see the per-type reference pages.

For validation errors, see [Error Codes](error-codes).

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
* [Repository Layout](project-setup) — how to organize the files referenced from the registry
