---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/recipes/force-republish.md
description: >-
  Manually trigger a rebuild and republish of a widget, script, or stylesheet
  without a code change
---

# Force Republish

Trigger a rebuild of your widgets, scripts, or stylesheets without modifying any extension code.

## When to Use This

* Content seems stale or outdated
* Build status shows success but your extensions aren't updating
* You need to refresh your extensions after platform updates
* Testing the publishing pipeline

## Prerequisites

* GitHub repository connected and enabled
* Widget currently publishing successfully

## Method 1: Empty Commit (Recommended)

The simplest way to trigger a republish:

### Steps

```bash
# Make sure you're on the watched branch
git checkout main

# Create an empty commit
git commit --allow-empty -m "Trigger widget rebuild"

# Push to trigger publishing
git push
```

### Verify

1. Go to **Integrations** → **Developer Studio** → **Sources**
2. Watch for build status to change to "In Progress"
3. Wait for ✓ Completed status
4. Refresh your page to see your updated widgets, scripts, or stylesheets

## Method 2: Toggle Repository

Disable and re-enable the repository:

### Steps

1. Go to **Integrations** → **Developer Studio** → **Sources**
2. Find your repository
3. Click the **Disable** toggle
4. Wait a few seconds
5. Click the **Enable** toggle
6. Wait for the build to complete

> **Note**: This temporarily removes widgets from the library while disabled.

## Method 3: Touch a File

Make a trivial change to force a new commit:

```bash
# Add a comment or whitespace to registry
echo "" >> extensions_registry.json

# Commit and push
git add extensions_registry.json
git commit -m "Trigger rebuild"
git push
```

> **Caution**: This modifies your file. Use Method 1 (empty commit) to avoid changes.

## Troubleshooting

### Build completes but widgets still old

1. **Browser cache**: Hard refresh (Ctrl+Shift+R / Cmd+Shift+R)
2. **Try incognito**: Open in private/incognito window
3. **Wait for propagation**: Allow 1-2 minutes for content to propagate
4. **Check version**: Verify widget version in registry matches expected

### Build fails after republish

1. Check the error message (hover over status icon)
2. Validate `extensions_registry.json` is valid JSON
3. Ensure all referenced content files exist
4. Review [Common Issues](../v2/common-issues)

### Empty commit not triggering build

Ensure you're pushing to the correct branch:

```bash
# Check which branch you're on
git branch --show-current

# Check which branch is being watched
# (visible in Sources settings)
```

## Quick Reference

| Method | Modifies Files | Downtime | Best For |
|--------|----------------|----------|----------|
| Empty commit | No | None | Most cases |
| Toggle repository | No | Brief | Quick refresh |
| Touch file | Yes | None | When empty commit fails |

## Related

* [Build & Publish](../v2/build-and-publish) — Understand what triggers builds and how they run
* [Preview and Promote](preview-and-promote) — Test changes on a staging branch before production
* [Common Issues](../v2/common-issues) — Troubleshoot build and publishing problems
