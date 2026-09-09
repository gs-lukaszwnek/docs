---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/first-script.md
description: >-
  A hands-on tutorial that walks you through creating, registering, and
  publishing a global script from scratch
---

# Your First Script

In this tutorial, you will create a global script from scratch, register it in `extensions_registry.json`, and publish it to your community. By the end, you will have a script that runs on every page and logs a message to the browser console.

## Before You Start

* You have a GitHub account.
* You have access to Sources in your platform (**Integrations** → **Developer Studio** → **Sources**).
* You have a connected GitHub organization — see [Connect Your GitHub Account](connect-github) if you haven't connected one yet.
* You have a repository from the [template](https://github.com/gainsight-hub/widgets-repository-template), enabled in Sources with `main` as the watched branch — see [Repository Layout](project-setup) and [Repository & Branch Settings](repository-settings).

## Step 1: Create the Script File

In your repository, create the directory `scripts/hello/` and inside it create `script.js` with this content:

```javascript
console.log('Hello from your first global script!')
```

This is a plain JavaScript file — no module syntax, no SDK. Global scripts run directly in the page context, so anything you would write in a normal `<script>` tag works here.

## Step 2: Register the Script

Open `extensions_registry.json` at the root of your repository and add this entry to the `scripts` array:

```json
{
  "scripts": [
    {
      "name": "hello",
      "path": "scripts/hello/script.js",
      "description": "Logs a greeting to the console on every page",
      "placement": "head"
    }
  ]
}
```

The `name` field is the script's unique identifier within the `scripts` array. The `path` points to the file you just created, relative to the repository root. `placement` tells the platform where to inject the `<script>` tag — `head` runs the script as early as possible.

## Step 3: Push and Publish

Commit and push your changes:

```bash
git add extensions_registry.json scripts/hello/script.js
git commit -m "Add hello script"
git push
```

Go back to **Integrations** → **Developer Studio** → **Sources** and watch the build status for your repository. After a few seconds it will change to **Completed**.

If the status shows **Failed**, open the build details to see the error message. The most common cause at this stage is a JSON syntax error in `extensions_registry.json`.

## Step 4: Verify It Runs

1. Open any page in your community.
2. Open the browser developer tools (F12 or right-click → **Inspect**).
3. Switch to the **Console** tab.

You should see `Hello from your first global script!` logged to the console. Navigate between pages — the message is logged on each one, because the script is injected into every page by default.

If you do not see the log, check that the build completed successfully and that the page is from the community where your repository is enabled.

## Step 5: Load the Script Only on Specific Pages

A script with no `rules` runs everywhere. To restrict it, add a `rules` array. Update your script entry so it only loads on the homepage:

```json
{
  "scripts": [
    {
      "name": "hello",
      "path": "scripts/hello/script.js",
      "description": "Logs a greeting to the console on the homepage",
      "placement": "head",
      "rules": [
        { "field": "page", "operator": "eq", "value": "homepage" }
      ]
    }
  ]
}
```

Commit, push, and wait for the build to complete. Reload your community's homepage — you should still see the console message. Navigate to any other page — the message is gone.

For the full list of targeting fields and operators, see [Page Targeting](page-targeting).

## What You Built

You created a JavaScript file, registered it in the platform, pushed it to production, and verified it runs on community pages. You then added a rule to target a specific page. The script runs directly in the page context with no SDK, on every request that matches its rules.

## What's Next

* [Scripts Overview](scripts-overview) — mental model for how scripts are injected
* [Script Definition Reference](scripts) — every field and HTML attribute
* [Page Targeting](page-targeting) — all context fields and operators for conditional loading
* [Your First Stylesheet](first-stylesheet) — add global CSS using the same registry pattern
