---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/first-stylesheet.md
description: >-
  A hands-on tutorial that walks you through creating, registering, and
  publishing a global stylesheet from scratch
---

# Your First Stylesheet

In this tutorial, you will create a global stylesheet from scratch, register it in `extensions_registry.json`, and publish it to your community. By the end, you will have a stylesheet that applies sitewide.

## Before You Start

* You have a GitHub account.
* You have access to Sources in your platform (**Integrations** → **Developer Studio** → **Sources**).
* You have a connected GitHub organization — see [Connect Your GitHub Account](connect-github) if you haven't connected one yet.
* You have a repository from the [template](https://github.com/gainsight-hub/widgets-repository-template), enabled in Sources with `main` as the watched branch — see [Repository Layout](project-setup) and [Repository & Branch Settings](repository-settings).

## Step 1: Create the Stylesheet File

In your repository, create the directory `stylesheets/highlight/` and inside it create `style.css` with this content:

```css
body {
  outline: 2px dashed #39a2ff;
  outline-offset: -6px;
}
```

This is a plain CSS file. Global stylesheets apply directly to the page — they are not scoped like widget styles — so anything you would write in a normal `<link rel="stylesheet">` works here.

## Step 2: Register the Stylesheet

Open `extensions_registry.json` at the root of your repository and add this entry to the `stylesheets` array:

```json
{
  "stylesheets": [
    {
      "name": "highlight",
      "path": "stylesheets/highlight/style.css",
      "description": "Adds a visible outline around the page body"
    }
  ]
}
```

The `name` field is the stylesheet's unique identifier within the `stylesheets` array. The `path` points to the file you just created, relative to the repository root.

## Step 3: Push and Publish

Commit and push your changes:

```bash
git add extensions_registry.json stylesheets/highlight/style.css
git commit -m "Add highlight stylesheet"
git push
```

Go back to **Integrations** → **Developer Studio** → **Sources** and watch the build status for your repository. After a few seconds it will change to **Completed**.

If the status shows **Failed**, open the build details to see the error message. The most common cause at this stage is a JSON syntax error in `extensions_registry.json`.

## Step 4: Verify It Applies

Open any page in your community. You should see a dashed blue outline around the page body, inset by 6 pixels. Navigate between pages — the outline appears on each one, because the stylesheet is loaded on every page by default.

If you do not see the outline, check that the build completed successfully and that the page is from the community where your repository is enabled.

## Step 5: Load the Stylesheet Only on Specific Pages

A stylesheet with no `rules` loads everywhere. To restrict it, add a `rules` array. Update your stylesheet entry so it only loads on the homepage:

```json
{
  "stylesheets": [
    {
      "name": "highlight",
      "path": "stylesheets/highlight/style.css",
      "description": "Adds a visible outline, homepage only",
      "rules": [
        { "field": "page", "operator": "eq", "value": "homepage" }
      ]
    }
  ]
}
```

Commit, push, and wait for the build to complete. Reload your community's homepage — you should still see the outline. Navigate to any other page — the outline is gone.

For the full list of targeting fields and operators, see [Page Targeting](page-targeting).

## What You Built

You created a CSS file, registered it in the platform, pushed it to production, and verified it applies to community pages. You then added a rule to target a specific page. The stylesheet loads via a standard `<link>` tag on every request that matches its rules.

## What's Next

* [Stylesheets Overview](stylesheets-overview) — mental model for how stylesheets are injected
* [Stylesheet Definition Reference](stylesheets) — every field and HTML attribute
* [Page Targeting](page-targeting) — all context fields and operators for conditional loading
* [Your First Script](first-script) — add global JavaScript using the same registry pattern
