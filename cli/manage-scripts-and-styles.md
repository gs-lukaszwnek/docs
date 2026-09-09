---
url: https://developer-portal.gainsight.com/docs/cli/manage-scripts-and-styles.md
description: >-
  Register sitewide scripts and stylesheets from the CLI, either as local files
  or as external hosted URLs, and scope them to specific pages.
---

# Manage Scripts and Stylesheets

Use this guide when you want to add a sitewide script or stylesheet — a snippet that runs on every rendered page of your community, or on a specific page — and register it with `gsds` so the change is tracked in `extensions_registry.json`.

Scripts and stylesheets are **Extensions** alongside Custom Widgets. Unlike widgets, they inject globally on the page rather than rendering inside the No-Code Builder.

## Choose a subcommand

`gsds script` and `gsds style` share the same shape:

| You want to... | Use |
|---|---|
| Create a local file in your repository | `gsds script new` or `gsds style new` |
| Register an external hosted URL | `gsds script link` or `gsds style link` |
| List everything currently registered | `gsds script ls` or `gsds style ls` |
| Remove an entry | `gsds script rm` or `gsds style rm` |

## Add a local script or stylesheet

```sh
gsds script new analytics-tracker
gsds style new brand-overrides
```

`new` scaffolds the source file in your repository and adds an entry to the registry. Edit the file and re-run `gsds build` to update the registry.

## Register an external URL

```sh
gsds script link https://cdn.example.com/analytics/v1/tracker.js --name analytics-tracker
gsds style link https://cdn.example.com/themes/brand.css
```

`link` registers a hosted URL rather than a local file. The URL is the positional argument and must end in `.js` for scripts and `.css` for stylesheets. Pass `--name` to override the slug that `gsds` derives from the URL.

## Common flags on `new` and `link`

| Flag | Purpose |
|---|---|
| `--attr k=v` | Add an HTML attribute to the injected tag (repeatable) |
| `--page <name>` | Scope the asset to a specific page instead of the whole site |
| `--placement head\|bodyStart\|bodyEnd` | Where to inject the script tag. Scripts only |
| `--name <slug>` | Explicit slug. Available on `link` only |

## Scope an asset to one page

Sitewide is the default. Pass `--page` to inject on a specific page only:

```sh
gsds script new page-analytics --page product-tour --placement bodyEnd
```

## List and remove entries

```sh
gsds script ls
gsds style ls
```

Removing a named entry:

```sh
gsds script rm analytics-tracker
gsds style rm brand-overrides
```

`rm` refuses to delete a folder with contents. Pass `--force` to delete a non-empty folder.

## Handle registry drift

If a registered script or stylesheet points at a file that no longer exists, `gsds build` refuses to regenerate the registry until you fix it. You have two ways out:

* Restore the missing file, or
* Remove the stale entry with `gsds script rm <name>` or `gsds style rm <name>`

`gsds create` warns on the same drift without failing so you can still scaffold new widgets while cleaning up.

## Next steps

* Rebuild registries and validate for CI in the [Command reference](reference/commands)
* Understand what's stored where in [Project files](reference/project-files)
* Resolve the `Script "x" points at missing file` message in [Troubleshooting](reference/troubleshooting)
