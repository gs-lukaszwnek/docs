---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/common-issues.md
description: >-
  Symptom-based troubleshooting when there's no specific error code — rate
  limiting, content safety rejections, Content Security Policy errors, content
  propagation delays, and installation suspension
---

# Common Issues

This guide covers common issues you may encounter with custom widgets — including build failures, content safety rejections, Content Security Policy errors, content propagation delays, and installation suspension — and how to resolve them.

## Prerequisites

* Familiarity with [Connect Your GitHub Account](connect-github)
* Understanding of [Repository & Branch Settings](repository-settings)

## Session and Rate Limiting Issues

### Connection keeps failing after multiple attempts

**Root cause**: Repeated connection attempts in quick succession can trigger rate limiting.

**Solution**:

1. Stop trying to connect
2. Wait at least one hour
3. Clear your browser cache and cookies
4. Try again with a fresh browser session

### OAuth flow suddenly stopped working

**Root cause**: Your connection session expired. Sessions are valid for approximately 30 minutes.

**Solution**:

1. Return to Sources settings
2. Click **Manage Accounts** to start fresh
3. Complete the entire flow in one session

> **Tip**: Keep the connection flow tab active and don't navigate away during the process.

## Connection Issues

### My organization doesn't appear in the picker

**Possible causes:**

1. You're not a member of that organization on GitHub
2. The GitHub App isn't installed on that organization
3. Your organization requires admin approval

**Solutions:**

1. **Verify membership**: Go to GitHub and confirm you're a member of the organization
2. **Install the app**: Click "Install on New Account" to add the app to your organization
3. **Request approval**: If you see "Request" instead of "Install", your org requires admin approval — see [Admin Approval](admin-approval)

### OAuth error page displayed

**Possible causes:**

* Session expired during authorization
* Browser cookies blocked
* Conflicting GitHub sessions
* GitHub API rate limiting (too many requests)

**Solutions:**

1. **Clear session**: Sign out of GitHub completely, then sign back in
2. **Clear cookies**: Clear browser cookies for both GitHub and this platform
3. **Try incognito**: Use an incognito/private browser window
4. **Check extensions**: Disable browser extensions that might interfere with OAuth
5. **Wait and retry**: If rate limited, wait a few minutes before trying again

### Connection times out

**Possible causes:**

* Network issues
* GitHub API unavailable
* Server-side processing delay

**Solutions:**

1. **Check internet**: Verify your internet connection is working
2. **Check GitHub status**: Visit [githubstatus.com](https://www.githubstatus.com) for outages
3. **Retry**: Wait a few minutes and try again
4. **Talk to your Gainsight team**: If timeouts persist, talk to your Gainsight team

### How do I remove a GitHub account from my community

Use **Disconnect** on the Manage GitHub accounts page. Open this page via **Manage Accounts** from Sources. That only removes the link for your community; it does not uninstall the app from GitHub. The account will appear under "Available to connect" if you want to reconnect later. To remove the app from the organization entirely (affects all communities), see [Multiple Organizations](multiple-organizations#option-b-uninstall-the-app-from-github) — Option B.

### I disconnected by mistake

Click **Manage Accounts** again. The account appears under **Available to connect**. Select it and click **Connect Selected** to reconnect.

## Repository Issues

### Repository not showing after connection

**Possible causes:**

* App doesn't have access to that repository
* Repository was added after app installation
* Cache not refreshed

**Solutions:**

1. **Check app permissions**: Go to GitHub → Organization Settings → Installed Apps → Configure and verify repository access
2. **Grant access**: Add the missing repository to the app's access list
3. **Refresh**: Click the refresh button or reload the page
4. **Reconnect**: Click **Manage Accounts** again to refresh your connection

### Repository was working but suddenly stopped updating

**Possible causes**:

1. The watched branch was deleted on GitHub
2. The GitHub App was removed from the repository's access list
3. The installation was suspended

**Solutions**:

1. **Check the branch exists**: Go to GitHub and verify the watched branch still exists in the repository
2. **Check app access**: Go to GitHub → Organization Settings → Installed Apps → Configure, and verify the repository is in the access list
3. **Check installation status**: If you see any suspension notices, contact your GitHub organization admin

### Build status stuck on Failed

**Possible causes:**

* Invalid widget configuration
* Missing required files
* Syntax errors in configuration
* Referenced assets not found

**Solutions:**

1. **Check error details**: In **Integrations** → **Developer Studio** → **Sources**, find the repository, and click the build status indicator to see the error details
2. **Validate configuration**: Check your `extensions_registry.json` against [Registry Reference](registry-reference) and the per-type references ([Widget Definition Reference](widget-schema), [Script Definition Reference](scripts), [Stylesheet Definition Reference](stylesheets)) to ensure all required fields are present and values are correct
3. **Check file paths**: Verify all referenced files exist in the repository
4. **Push a fix**: Correct the issue and push a new commit to retry

**Common configuration errors:**

| Error | Cause | Fix |
|-------|-------|-----|
| "Invalid JSON" | Syntax error in config | Run config through a JSON validator |
| "Missing required field" | Config incomplete | Add the missing field |
| "File not found" | Asset doesn't exist | Check file path or add the file |

### Some widgets publish but others fail

**Root cause**: Individual widget content files may be missing or inaccessible.

**Solution**:

1. Check the build status details to see which specific widgets failed
2. Verify each widget's `source.path` and `source.entry` point to an existing directory and file
3. Ensure all files are committed to the watched branch
4. Push a fix and the system will retry automatically

## Static Widget (Source Block) Issues

### Path traversal detected

**Error code**: `PATH_TRAVERSAL_DETECTED`

The `source.path` or `source.entry` contains `../`. See [Error Codes](error-codes#path-traversal-detected) for the cause and fix.

### Entry file not found

**Error code**: `ENTRY_FILE_NOT_FOUND`

The HTML file named in `source.entry` doesn't exist in `source.path`. See [Error Codes](error-codes#entry-file-not-found) for the cause and fix.

### Directory exceeds limits

**Error codes**: `MAX_FILES_EXCEEDED`, `MAX_SIZE_EXCEEDED`

The widget directory has more than 100 files or exceeds 10 MB. See [Error Codes](error-codes#max-files-exceeded) for the cause and fix.

### Invalid widget structure

**Error code**: `INVALID_WIDGET_STRUCTURE`

A widget has both `source` and `content` blocks, or neither — it must have exactly one. See [Error Codes](error-codes#invalid-widget-structure) for the cause and fix.

### Assets not loading in my widget

**Root cause**: JavaScript files dynamically loading assets use relative paths that aren't transformed.

**Solution**: Asset transformations only apply to HTML. For JS dynamic loading:

1. Use a base URL variable passed from HTML
2. Inline small assets as data URLs
3. Preload assets in HTML where paths get transformed

See [Hosting Widgets](hosting-widgets#javascript-dynamic-loading-limitation) for related configuration details.

For a complete list of error codes and resolution guidance, see [Error Codes](error-codes).

### Build fails with no visible error

**Root cause**: The `extensions_registry.json` file has a structural problem.

**Common issues**:

* Invalid JSON syntax (missing commas, unclosed braces)
* Missing `widgets` array at the root level
* `widgets` is not an array (e.g., it's an object or string)

**Solution**:

1. Validate your JSON using an online validator or IDE
2. Ensure the file structure is: `{"widgets": [...]}`
3. Check for trailing commas (not allowed in JSON)

## Scripts & Stylesheets Issues

These issues affect global scripts and stylesheets declared in the `scripts` and `stylesheets` arrays of `extensions_registry.json`, not widgets.

### Script or stylesheet doesn't load on the expected pages

**Root cause**: The entry's `rules` don't match the page you're viewing.

**Solutions**:

1. **All rules must match** — multiple rules use AND logic, so one rule that never matches keeps the asset off the page.
2. **Check the field is present on that page** — `page` is only set on customizable and overview pages, `pageType` only on category and custom pages, and `slug` only on custom pages. A rule against a field that's absent on the current page never matches.
3. **Match all categories with `pageType`** — for category and custom pages the `page` value always includes the ID suffix (e.g. `category/123`), so `page` equal to `category` never matches. Use `pageType` (`category` or `customPage`) instead.
4. **Use string values for `authenticated`** — match against `"true"` or `"false"`, not a boolean.
5. **Match the value shape to the operator** — use a string with `eq`/`neq` and an array with `in`/`not_in`.

See [Page Targeting](page-targeting) for the full rule language and context fields.

### An HTML attribute has no effect

**Root cause**: The `attributes` object maps each attribute name to a **string** value, and boolean attributes use an empty string rather than `true`.

**Solution**: Write boolean attributes like `defer` and `async` with an empty-string value:

```json
"attributes": { "defer": "" }
```

This renders as `<script src="..." defer></script>`. See [Script Definition Reference](scripts#html-attributes) and [Stylesheet Definition Reference](stylesheets#html-attributes) for the supported attributes.

### An external script or stylesheet URL doesn't load

**Root cause**: When `path` is an external URL, the platform injects a tag pointing straight at that URL — it does not fetch, publish, or proxy the file. Whether it loads depends entirely on the external host.

**Solutions**:

1. **Confirm the URL is reachable** — open it directly in a browser and verify it serves the file.
2. **Keep the required extension** — `path` must end in `.js` for scripts and `.css` for stylesheets even for external URLs, or the entry fails validation.
3. **Handle CORS if needed** — if the external host requires it, set the `crossorigin` attribute (`"anonymous"` or `"use-credentials"`).

## Widget Runtime Issues

These issues occur after a widget is published and running in the browser, not during the build phase.

### Widget shows 'Loading...' but connector executes successfully

**Root cause**: `document.querySelector()` cannot reach elements inside Shadow DOM. The connector response arrives, but your code fails to find the DOM element to update.

**Solution**: Query elements through the widget's shadow root, not `document`:

```js
var hosts = document.querySelectorAll(
  'gs-cc-registry-widget[data-widget-type*="your_widget_type"]'
);
hosts.forEach(function(host) {
  var root = host.shadowRoot;
  if (root) {
    var el = root.querySelector('.status');
    // update el here
  }
});
```

See [Rendering & DOM](rendering-and-dom) for the full pattern.

### Only one widget instance updates, others stay on 'Loading...'

**Root cause**: Using `querySelector` (returns the **first** match only) instead of `querySelectorAll`.

**Solution**: Use `querySelectorAll` and iterate over all host elements:

```js
// Wrong: querySelector returns only the first match
var host = document.querySelector('gs-cc-registry-widget[data-widget-type*="weather"]');

// Correct: querySelectorAll iterates all instances
var hosts = document.querySelectorAll('gs-cc-registry-widget[data-widget-type*="weather"]');
hosts.forEach(function(host) { /* update each instance */ });
```

See [Rendering & DOM](rendering-and-dom#multiple-widget-instances).

## Content Security Issues

### Widget rejected due to a policy violation

**Error code**: `CONTENT_SCAN_REJECTED`

**Cause**: Your widget code contains patterns that match a content safety rule. The error message includes the specific categories.

**Resolution by category**:

| Category | What to change |
|----------|---------------|
| `crypto_mining` | Remove cryptocurrency mining scripts entirely |
| `data_exfiltration` | Remove code that sends user data to external servers. If you need to send data, use a [Connector](/connectors/) instead |
| `phishing_redirect` | Remove fake login forms or deceptive redirects. Use your platform's built-in authentication |
| `obfuscation` | Replace obfuscated code with readable source. If you use a bundler, check that its output is not overly minified |
| `external_script_loading` | Load scripts from trusted sources only, or bundle dependencies into your widget directory |
| `credential_exposure` | Remove hardcoded secrets from your code. Use environment variables, a secrets manager, or [Connectors](/connectors/) to handle credentials securely |

See [Content Security](content-security) for full details.

### My widget breaks after security headers were added

**Cause**: The Content Security Policy restricts certain code patterns for security.

| Symptom | Cause | Fix |
|---------|-------|-----|
| `eval()` throws an error | CSP blocks `eval()` and `new Function()` | Rewrite code to avoid runtime evaluation |
| External script fails to load | CSP restricts script sources to `'self'`, inline, and trusted CDNs (see [Content Security](content-security)) | Load scripts from trusted CDNs or bundle them into your widget directory |
| Styles not applying | Unlikely — inline styles are allowed | Check for `@import` from external domains |
| Widget not rendering in an iframe | `X-Frame-Options` limits embedding | Widgets render within the platform's frame — this is expected behavior |

## Publishing Issues

### Widgets not updating after push

**Checklist:**

1. **Correct branch?** — Push must be to the **watched branch**
   * Check which branch is configured in repository settings
   * Pushes to other branches don't trigger updates

2. **Repository enabled?** — Disabled repos don't sync
   * Verify the repository is enabled in settings

3. **Build successful?** — Failed builds don't publish
   * Check build status for errors

4. **Content propagation** — Updates may take a few minutes
   * Wait 2-3 minutes for content to propagate

5. **Browser cache** — Old content may be cached locally
   * Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
   * Or try incognito mode

### Wrong content being served

**Possible causes:**

* Watching the wrong branch
* Cache serving old content
* Build failed silently

**Solutions:**

1. **Verify branch**: Check that you're watching the intended branch
2. **Clear cache**: Hard refresh or clear browser cache
3. **Check build**: Verify the latest build completed successfully
4. **Wait for propagation**: Updates can take a few minutes to propagate

### Content appears corrupted or incomplete

**Solutions:**

1. **Check source files**: Verify files in repository are correct
2. **Re-trigger build**: Push an empty commit to force a new build
   ```bash
   git commit --allow-empty -m "Trigger rebuild"
   git push
   ```
3. **Check encoding**: Ensure files use UTF-8 encoding

### Fewer extensions published than expected

**Root cause**: A successful build **skips** extensions that don't need republishing, so the published count can be lower than the number you declared.

An extension is skipped when:

* Its content hasn't changed since the last publish
* It uses an external URL endpoint, which the platform doesn't host

Skipped extensions are not an error — they didn't need updating. Check the build status details to see which were skipped.

## Installation Issues

If your installation appears suspended, see [Suspension Issues](#suspension-issues) for diagnosis and reactivation steps.

### All my organizations disappeared

**Root cause**: Your GitHub authentication may have expired or been revoked.

**Solution**:

1. Click **Manage Accounts** again to refresh your authentication
2. Re-authorize the application if prompted
3. Your organizations should reappear in the picker

## Suspension Issues

### Installation suspended by GitHub

GitHub may suspend app installations for various reasons:

**Common causes:**

* Organization billing issues
* Policy violations
* Admin action

**Solutions depend on who suspended the installation:**

* **If suspended by GitHub**: Check GitHub notifications for the reason, then resolve any billing or policy issues with GitHub support. After resolution, you may need to reinstall the app.
* **If suspended by your organization admin**: Ask the admin for details. They can see the suspension reason in GitHub's organization settings and reactivate in Settings → Integrations → Applications.

If you are unsure which applies, contact your GitHub organization admin first — they can see the suspension reason and act directly.

### How to reactivate a suspended installation

1. Resolve the underlying issue with GitHub (billing, policy, or admin action)
2. Go to GitHub Organization Settings → Installed Apps
3. Find the app and reactivate or reinstall
4. Return to your Sources and reconnect if needed

## Authentication Issues

### Suddenly lost access to connected organizations

**Possible causes:**

* GitHub token expired
* Removed from organization on GitHub
* App permissions changed

**Solutions:**

1. **Reconnect**: Try connecting to GitHub again to refresh tokens
2. **Check membership**: Verify your GitHub organization memberships
3. **Review permissions**: Check app permissions in GitHub settings

### Getting 'Forbidden' errors

**Solutions:**

1. **Refresh connection**: Click **Manage Accounts** again to refresh your connection
2. **Check permissions**: Verify the app has required permissions in GitHub
3. **Clear session**: Sign out and back in

## Performance Issues

### Builds taking too long

**Normal build times:**

* Small repos: A few seconds
* Medium repos: Under a minute
* Large repos: A few minutes

**If builds are unusually slow:**

1. **Check repo size**: Very large repos take longer
2. **Check GitHub status**: GitHub API slowdowns affect build times
3. **Stuck In Progress**: If a build stays In Progress for more than 10 minutes, talk to your Gainsight team
4. **Talk to your Gainsight team**: Persistent slowness may indicate an issue

### Frequent build failures

**Solutions:**

1. **Review patterns**: Look for common causes in your build history in **Integrations** → **Developer Studio** → **Sources**
2. **Validate locally**: Run your `extensions_registry.json` through a JSON validator and review [Registry Reference](registry-reference)
3. **Check dependencies**: Ensure all files referenced in `source.path` and `source.entry` exist in the repository

## Widget Development Issues

### Widget renders nothing

* Verify the `<div id="root"></div>` (or your chosen mount point) exists in your widget HTML template.
* Ensure you are querying with `sdk.root.querySelector(...)`, not `document.querySelector(...)`. Your widget lives in a Shadow DOM.

### Styles don't apply

* Styles in your widget HTML template are automatically scoped to the Shadow DOM by the platform.
* External stylesheets work but must be referenced from within your template.
* Host page styles do **not** penetrate the Shadow DOM (this is by design).

### Props are empty or undefined

* Check that the widget's configuration contains valid JSON.
* Props are read at call time — call `sdk.getProps()` when you need the current values.

### Widget doesn't clean up properly

* Ensure you subscribe to `sdk.on('destroy', ...)` in your `init` function and tear down your UI framework.
* If using intervals or event listeners, clean them up in the destroy handler.

### Module import errors

* Verify your widget is bundled as an ES module (`format: 'es'` in your build config).
* If using a framework (React, Vue, etc.), ensure it is bundled into your widget output — the platform does not provide frameworks.

## Getting Help

If you can't resolve your issue:

1. **Gather information**:
   * Error code and message (screenshots help)
   * Steps to reproduce
   * Browser and OS version
   * Organization name (if applicable)

2. **Check documentation**:
   * [Widget Runtime](core-concepts) — How widgets work at runtime
   * [Connect Your GitHub Account](connect-github)
   * [Repository & Branch Settings](repository-settings)
   * [Build & Publish](build-and-publish)
   * [Admin Approval](admin-approval)

3. **Talk to your Gainsight team**:
   * Provide the gathered information
   * Include relevant build IDs or timestamps
   * Describe what you've already tried

## Quick Reference

| Symptom | First Check | Guide |
|---------|-------------|-------|
| Can't find organization | GitHub membership | [Connect Your GitHub Account](connect-github) |
| Repository not visible | App permissions | [Repository Settings](repository-settings) |
| Installation suspended | Org admin / GitHub status | This guide |
| Widget not updating | Watched branch | [Build & Publish](build-and-publish) |
| Build failing | Error messages | This guide |
| Need admin approval | Enterprise policies | [Admin Approval](admin-approval) |
| Static widget errors | Error code | [Error Codes](error-codes) |
| Widget stuck on "Loading..." | Shadow DOM query | [Rendering & DOM](rendering-and-dom) |
| Only one instance updates | querySelector vs querySelectorAll | [Rendering & DOM](rendering-and-dom#multiple-widget-instances) |
| Widget renders nothing / styles broken | Shadow DOM, mount point | This guide (Widget Development) |

## Next Steps

* [Error Codes](error-codes) — Full error code reference with causes and fixes
* [Build & Publish](build-and-publish) — Understand the push-to-publish pipeline
* [Content Security](content-security) — Learn about content scanning and security policies
