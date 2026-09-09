---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/error-codes.md
description: >-
  Look up a specific error code shown while building, validating, or publishing
  an extension (widget, script, or stylesheet) — causes and fixes indexed by
  code; for troubleshooting without a known error code, see Common Issues
---

# Error Codes

This page documents the error codes you may encounter when publishing extensions — widgets, scripts, and stylesheets — indexed by code, with the cause and fix for each.

Most named codes below apply to source widgets. Script and stylesheet configuration problems — an invalid field, a `path` with the wrong extension, or a malformed `rules` entry — surface as generic JSON-schema validation errors rather than a named error code. See [Script Definition Reference](scripts) and [Stylesheet Definition Reference](stylesheets) for their field rules.

## Error Code Overview

Each error includes:

* **Code**: Unique identifier for the error
* **Stage**: When the error occurs (validation, fetch, transformation, upload)
* **Retryable**: Whether the error may resolve on retry

## Validation Errors

These errors occur during schema validation and configuration checks. They are **not retryable** — you must fix the underlying issue.

### PATH\_TRAVERSAL\_DETECTED

**Message**: Path traversal detected in source.path/entry

**Cause**: The `source.path` or `source.entry` field contains `../` which could access files outside the widget directory.

**Fix**: Use relative paths without `../`:

```json
// Wrong
"source": {
  "path": "../shared/widget",
  "entry": "index.html"
}

// Correct
"source": {
  "path": "widgets/my-widget",
  "entry": "index.html"
}
```

***

### ABSOLUTE\_PATH\_NOT\_ALLOWED

**Message**: Absolute paths are not allowed in source.path

**Cause**: The `source.path` field starts with `/`, making it an absolute path.

**Fix**: Use a relative path from the repository root:

```json
// Wrong
"source": {
  "path": "/widgets/my-widget",
  "entry": "index.html"
}

// Correct
"source": {
  "path": "widgets/my-widget",
  "entry": "index.html"
}
```

***

### INVALID\_SOURCE\_SCHEMA

**Message**: Invalid source block schema

**Cause**: The `source` block is missing required fields or has invalid field types.

**Fix**: Ensure both `path` and `entry` are non-empty strings:

```json
"source": {
  "path": "widgets/my-widget",  // Required: string
  "entry": "index.html"         // Required: string
}
```

***

### ENTRY\_FILE\_NOT\_FOUND

**Message**: Entry file not found at source.path/source.entry

**Cause**: The HTML file specified in `source.entry` doesn't exist in the `source.path` directory.

**Fix**:

1. Verify the file exists in your repository
2. Check for typos in the path or filename
3. Ensure the file is committed to the watched branch

```
widgets/my-widget/
├── index.html  ← This file must exist
├── styles.css
└── app.js
```

***

### INVALID\_WIDGET\_STRUCTURE

**Message**: Widget structure is invalid — must have either source or content block

**Cause**: A widget definition has both `source` and `content` blocks, or neither. Each widget must define exactly one content delivery method.

**Fix**: Use exactly ONE of `source` or `content`:

```json
// Wrong: both blocks
{
  "source": { "path": "...", "entry": "..." },
  "content": { "endpoint": "...", "method": "GET" }
}

// Wrong: neither block
{
  "version": "1.0.0",
  "title": "My Widget"
  // Missing source AND content
}

// Correct: source only
{
  "source": {
    "path": "widgets/my-widget",
    "entry": "index.html"
  }
}

// Correct: content only (external URL)
{
  "content": {
    "endpoint": "https://example.com/widgets/my-widget.html",
    "method": "GET"
  }
}
```

***

### MAX\_FILES\_EXCEEDED

**Message**: Widget directory exceeds maximum file count (limit: 100)

**Cause**: The `source.path` directory contains more than 100 files.

**Fix**:

1. Remove unnecessary files from the widget directory
2. Combine or minify assets where possible
3. Host shared assets externally and reference them by URL
4. Talk to your Gainsight team if you have a legitimate need for more files

***

### MAX\_SIZE\_EXCEEDED

**Message**: Widget directory exceeds maximum size (limit: 10 MB)

**Cause**: The total size of all files in `source.path` exceeds 10 MB.

**Fix**:

1. Optimize images (compress, use WebP format)
2. Minify CSS and JavaScript
3. Remove unused assets
4. Host large assets externally and reference them by URL

***

### CONTENT\_SCAN\_REJECTED

**Message**: Widget rejected: content policy violation

**Cause**: The widget code contains patterns that match a content safety rule. The error message includes the specific categories that triggered the rejection.

**Possible categories**: `crypto_mining`, `data_exfiltration`, `phishing_redirect`, `obfuscation`, `external_script_loading`, `credential_exposure`

**Fix**:

1. Read the category in the error message
2. Follow the resolution steps in [Content Security — How to Resolve Each Category](content-security#how-to-resolve-each-category)
3. Push the corrected code to trigger a new build

***

## Platform Errors

These errors occur during external service interactions. They **may be retryable** — wait and push again, or talk to your Gainsight team if persistent.

### GITHUB\_FETCH\_FAILED

**Message**: Failed to fetch files from GitHub

**Cause**: Unable to download widget files from your GitHub repository.

**Possible reasons**:

* GitHub API temporarily unavailable
* Repository access revoked
* Network connectivity issues
* Rate limiting

**Fix**:

1. Check [GitHub Status](https://www.githubstatus.com) for outages
2. Verify repository access in GitHub app settings
3. Wait a few minutes and push again to trigger a retry
4. Talk to your Gainsight team if the error persists

***

### CDN\_UPLOAD\_FAILED

> This error code appears in your build logs and means the platform could not publish your widget content.

**Message**: Failed to publish widget content

**Cause**: Unable to publish widget content to the platform.

**Possible reasons**:

* Storage service temporarily unavailable
* Network issues

**Fix**:

1. Wait a few minutes and push again to trigger a retry
2. The system will automatically retry on infrastructure failures
3. Talk to your Gainsight team if the error persists after multiple attempts

***

### TRANSFORMATION\_FAILED

**Message**: Failed to transform HTML content

**Cause**: An error occurred while transforming asset references in the HTML entry file.

**Possible reasons**:

* Malformed HTML that can't be parsed
* Encoding issues

**Fix**:

1. Validate your HTML file
2. Ensure the file uses UTF-8 encoding
3. Check for unusual characters or invalid markup

## Error Stages

Each error includes a stage indicating when it occurred:

| Stage | Description | Typical Errors |
|-------|-------------|----------------|
| `validation` | Schema and configuration checks | PATH\_TRAVERSAL, INVALID\_WIDGET\_STRUCTURE, MAX\_FILES\_EXCEEDED |
| `fetch` | Downloading files from GitHub | GITHUB\_FETCH\_FAILED, ENTRY\_FILE\_NOT\_FOUND |
| `scan` | Content safety checks | CONTENT\_SCAN\_REJECTED |
| `transformation` | Processing HTML content | TRANSFORMATION\_FAILED |
| `upload` | Publishing content | CDN\_UPLOAD\_FAILED |

For step-by-step troubleshooting, see [Common Issues](common-issues).

## Getting Help

If you can't resolve an error, see [Common Issues](common-issues#getting-help).

## Next Steps

* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
* [Widget Definition Reference](widget-schema) — widget entry fields
* [Common Issues](common-issues) — General troubleshooting

Platform (may be retryable):

* GITHUB\_FETCH\_FAILED: could not download files from GitHub — check GitHub status and repo access
* CDN\_UPLOAD\_FAILED: could not publish widget content — wait and push again to retry
* TRANSFORMATION\_FAILED: error processing HTML asset references — validate HTML and encoding
