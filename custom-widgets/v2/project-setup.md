---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/project-setup.md
description: >-
  Required file and directory layout for a repository that publishes widgets,
  scripts, and stylesheets
---

# Repository Layout

This guide explains how to structure your GitHub repository for publishing extensions — widgets, scripts, and stylesheets. Build any combination of the three; every shape shares the same layout.

## Prerequisites

* A GitHub account
* Basic understanding of JSON

## Quick Start: Use the Template

The easiest way to get started is to fork the official template repository:

**<https://github.com/gainsight-hub/widgets-repository-template>**

1. Go to the template repository
2. Click **Fork** (or use as template)
3. Clone your forked repository
4. Customize the extensions for your needs

## Repository Structure

Your repository should follow this structure:

```
your-repo/
├── extensions_registry.json          # Required: Widget, script, and stylesheet definitions
├── connectors_registry.json      # Optional: Connector definitions
├── widgets/
│   ├── my_widget/
│   │   └── index.html            # Widget content file
│   └── another_widget/
│       └── index.html
├── scripts/
│   └── analytics/
│       └── script.js             # Global script files
└── stylesheets/
    └── theme/
        └── style.css             # Global stylesheet files
```

## Required File: `extensions_registry.json`

This file must be at the **root** of your repository. It contains the definitions for all your widgets, scripts, and stylesheets. For the root object, see [Registry Reference](registry-reference). For per-type entry fields, see [Widget Definition Reference](widget-schema), [Script Definition Reference](scripts), and [Stylesheet Definition Reference](stylesheets).

:::info Migration from `widget_registry.json`
If your repository uses the old `widget_registry.json` filename, it still works during the migration window. Rename it to `extensions_registry.json` at your earliest convenience — the old name is deprecated and will be removed in a future release. No other schema changes are required.
:::

```json
{
  "widgets": [ ... ],
  "scripts": [ ... ],
  "stylesheets": [ ... ]
}
```

## Widget Content Files

Each widget's `source` block points to the widget content in your repository. The `source.path` specifies the directory (relative to repository root) and `source.entry` specifies the HTML entry point within that directory.

**Supported content types:**

* HTML files with optional CSS, JavaScript, and image assets

**How content is served:**

* The entire `source.path` directory is published to the platform
* Asset references in the entry HTML file are automatically transformed to hosted URLs

### Asset Paths in Widget HTML

Paths in your widget HTML (`src`, `href`) are **relative to the widget's directory** — the directory specified in `source.path`. The platform publishes everything inside that directory and rewrites relative paths to hosted URLs automatically.

For example, given this registry entry:

```json
{
  "source": {
    "path": "widgets/greeting",
    "entry": "index.html"
  }
}
```

And this directory layout:

```
widgets/greeting/
├── index.html
├── app.js
└── icon.svg
```

Your `index.html` references files relative to its own directory:

```html
<script type="module" src="app.js"></script>
<img src="icon.svg" alt="icon" />
```

Use relative paths — they are the intended form and resolve against the widget directory. Leading slashes are stripped during publishing (so `/app.js` resolves the same as `app.js`), but relative paths make the intent clearer and match how the files are laid out on disk.

## Script and Stylesheet Files

### Directory Conventions

Place each script and stylesheet in its own directory for clarity:

```
scripts/
├── analytics/
│   └── script.js
├── hero-tracking/
│   └── script.js
└── external-lib/
    └── script.js

stylesheets/
├── theme/
│   └── style.css
├── components/
│   └── style.css
└── responsive/
    └── style.css
```

### File Naming

* Scripts: `{name}/script.js` (always `script.js`)
* Stylesheets: `{name}/style.css` (always `style.css`)
* The directory name becomes the identifier in your registry

### External vs Repository-Hosted

**Repository-hosted** (path is relative):

```json
{
  "name": "analytics",
  "path": "scripts/analytics/script.js"
}
```

**External** (path is a URL):

```json
{
  "name": "external-analytics",
  "path": "https://cdn.example.com/analytics.js"
}
```

## How It Works

1. You push changes to your watched branch
2. The platform fetches `extensions_registry.json` from your repository
3. If `connectors_registry.json` exists, connectors are validated and synced
4. For each widget with a `source` block, the widget directory is fetched and published
5. For each script or stylesheet with a relative path, the file is fetched and published
6. External URLs for scripts/stylesheets are used as-is
7. Widgets, scripts, and stylesheets become available in the No-Code Builder and on community pages

## Empty Registries

An empty `widgets` array is valid:

```json
{
  "widgets": [],
  "scripts": [],
  "stylesheets": []
}
```

This will:

* Pass validation successfully
* Show "Completed" build status
* Publish 0 extensions (no content uploaded)

This is useful when you want to:

* Temporarily remove all content without disabling the repository
* Start with a clean slate before adding extensions
* Test that your repository connection works

## Removing Extensions

When you remove a definition from the registry and push to the watched branch, that extension is removed from your Community.

* Widget: Removed if its `type` no longer appears in the `widgets` array
* Script: Removed if its `name` no longer appears in the `scripts` array
* Stylesheet: Removed if its `name` no longer appears in the `stylesheets` array

:::warning
Removing extensions is a destructive action. If a widget is already placed on pages in the No-Code Builder, those pages will show a missing widget. Make sure the extension is no longer in use before removing it.
:::

## Template Repository Features

The [template repository](https://github.com/gainsight-hub/widgets-repository-template) includes:

* **Example widgets**: Ready-to-use demo widgets
* **Example scripts and stylesheets**: Sample global assets with default configurations
* **Build script**: `bin/build-registry.sh` to generate registry from individual configs
* **Documentation**: Detailed setup guide in `widgets/WIDGET_SETUP.md`
* **Default config**: Global settings in `config/defaults.json`

## Best Practices

1. **Use the template**: Start with the template repo to avoid common mistakes
2. **Keep organized**: One folder per extension under `widgets/`, `scripts/`, or `stylesheets/`
3. **Use relative paths**: For repository-hosted files, define paths relative to repo root
4. **Version your widgets**: Update the `version` field when making changes
5. **Validate JSON**: Ensure `extensions_registry.json` is valid JSON before pushing
6. **Test with empty registry first**: Verify your connection works before adding extensions
7. **Name extensions uniquely**: Across all connected repositories, extension names must be unique

## Troubleshooting

### "Widget registry file is invalid"

* Ensure `extensions_registry.json` is at the repository root
* Validate the JSON syntax (use a JSON validator online)
* Check that all required fields are present

### "Content file not found"

* Verify that `source.path` and `source.entry` are correct for widgets
* Verify that script/stylesheet paths are correct
* Paths are relative to the repository root
* Check that the entry file exists in the watched branch

### "Script path error" or "Stylesheet path error"

* Ensure `path` ends in `.js` (scripts) or `.css` (stylesheets)
* For repository files, use paths like `scripts/analytics/script.js`
* For external files, use full URLs starting with `http://` or `https://`

### "Build failed after push"

* Check if `extensions_registry.json` passes schema validation
* Ensure all referenced files exist in the watched branch
* Review build error details by hovering over the status icon
* See [Common Issues](common-issues) for specific error codes

## Next Steps

* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
* [Widget Definition Reference](widget-schema) — widget entry fields
* [Script Definition Reference](scripts) — script entry fields
* [Stylesheet Definition Reference](stylesheets) — stylesheet entry fields
* [Configurable Widgets](configurable-widgets) — Let editors customize widgets
* [Build & Publish](build-and-publish) — How publishing works
