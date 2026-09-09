---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/hosting-widgets.md
description: Choose how to host your widget — externally at a URL or in your repository
---

# Hosting Widgets

Your widget content can be hosted in two ways: **externally** (served from a URL you control) or **in your repository** (published by the platform). See [Choosing an Approach](#choosing-an-approach) for guidance on which option fits your workflow. The sections below document the configuration for each. Scripts and stylesheets follow the same hosting model — see [Scripts Overview](scripts-overview), [Stylesheets Overview](stylesheets-overview), and [Repository Layout](project-setup).

## External Hosting

Use the **content block** when your widget is hosted at a public URL. The platform fetches HTML and assets from that URL whenever the widget is needed. See [Content Object](widget-schema#content-object) for the full field reference.

### Cache Strategies

| Value | Behavior |
|-------|----------|
| `"none"` | No caching, content fetched fresh each time |
| `"no-cache"` | Same as none, no caching |
| `"ttl"` | Cache for specified duration (use with `cacheTtlSeconds`) |
| `"permanent"` | Long-term caching |

`"none"` and `"no-cache"` are equivalent — both fetch content fresh on every request. Use either.

When using `"ttl"`, also specify `cacheTtlSeconds`:

```json
"content": {
  "endpoint": "https://example.com/widgets/my_widget.html",
  "method": "GET",
  "cacheStrategy": "ttl",
  "cacheTtlSeconds": 300
}
```

### External Hosting Example

```json
{
  "widgets": [
    {
      "version": "2.1.0",
      "title": "User Stats",
      "type": "my_company_user_stats",
      "description": "Shows user activity statistics from an external service",
      "category": "analytics",
      "containers": ["Left container", "Sidebar"],
      "widgetsLibrary": true,
      "settings": {
        "configurable": false,
        "editable": false,
        "removable": true,
        "shared": true,
        "movable": true
      },
      "content": {
        "endpoint": "https://stats.example.com/widgets/user_stats.html",
        "method": "GET",
        "cacheStrategy": "ttl",
        "cacheTtlSeconds": 300
      }
    }
  ]
}
```

## Repository Hosting

Use the **source block** when your widget content is kept locally in your repository. The platform publishes the entire directory you specify and transforms asset references to hosted URLs automatically. See [Source Block](widget-schema#source-block) for the full field reference.

**Example:**

```json
{
  "source": {
    "path": "widgets/my-static-widget",
    "entry": "index.html"
  }
}
```

### How Static Widgets Work

1. **Directory Upload**: The entire `source.path` directory is published to the platform
2. **HTML Transformation**: The `source.entry` file has asset references automatically transformed to hosted URLs
3. **Relative Path Preservation**: Directory structure is preserved after publishing

**Supported transformations:**

* HTML attributes: `src`, `href`, `data`, `poster` (in `<link>`, `<img>`, `<script>`, `<video>`, `<audio>`, `<source>`, `<embed>`, `<object>`)
* Inline styles: `url()` references in `style` attributes and `<style>` blocks
* Responsive images: `srcset` attributes with multiple image sources
* Root-relative paths: leading `/` is stripped before lookup, so `/assets/style.css` is treated the same as `assets/style.css` (useful for Vite/React build output)

### Repository Hosting Example

**Repository structure:**

```
widgets/
└── dashboard/
    ├── index.html
    ├── styles.css
    ├── app.js
    └── images/
        ├── logo.png
        └── background.jpg
```

**Registry configuration:**

```json
{
  "widgets": [
    {
      "version": "1.0.0",
      "title": "Dashboard Widget",
      "type": "my_company_dashboard",
      "description": "Interactive dashboard with custom styling",
      "category": "analytics",
      "containers": ["Full width"],
      "widgetsLibrary": true,
      "settings": {
        "configurable": true,
        "removable": true
      },
      "source": {
        "path": "widgets/dashboard",
        "entry": "index.html"
      }
    }
  ]
}
```

**HTML file (`widgets/dashboard/index.html`) — plain relative paths:**

```html
<link rel="stylesheet" href="styles.css">
<img src="images/logo.png" alt="Logo">
<script src="app.js"></script>
```

**HTML file built by Vite/React — root-relative paths:**

```html
<link rel="stylesheet" href="/assets/index-abc123.css">
<script type="module" src="/assets/index-abc123.js"></script>
```

After publishing, all relative paths and root-relative paths are automatically transformed to hosted URLs. Root-relative paths (starting with `/`) are resolved against the widget's published directory, so `/assets/index-abc123.js` is treated the same as `assets/index-abc123.js`.

### Directory Limits

Static widgets have configurable limits to ensure performance:

| Limit | Default | Description |
|-------|---------|-------------|
| Maximum files | 100 | Total file count in the directory |
| Maximum size | 10 MB | Total size of all files combined |

If your widget exceeds these limits, the build will fail with a clear error message.

### Path Security

For security, these restrictions apply to the `source.path` and `source.entry` configuration fields:

* **No path traversal**: `path` and `entry` cannot contain `../`
* **No absolute paths**: `path` and `entry` cannot start with `/`
* **Symlinks skipped**: Symbolic links are ignored during processing

> **Note:** These restrictions apply only to the `source.path` and `source.entry` fields in `extensions_registry.json`. URLs inside your HTML content files (such as `/assets/style.css`) are not restricted — root-relative URLs are automatically transformed to hosted URLs during publishing.

### JavaScript Dynamic Loading Limitation

::: warning Known Limitation
Asset URLs in JavaScript files are **not** transformed. If your JS dynamically loads assets using relative paths, those paths will break after publishing.
:::

**Workarounds:**

1. **Inline the assets**: Use data URLs for small assets
   ```javascript
   // Instead of: const logo = 'images/logo.png';
   const logo = 'data:image/png;base64,...';
   ```

2. **Use a base URL variable**: Pass the widget base URL from HTML to JS

   ::: warning
   `document.currentScript` may be `null` in the widget execution context (scripts run inside Shadow DOM). Test this pattern before relying on it. For a reliable alternative, see [Rendering & DOM](rendering-and-dom#script-context).
   :::

   ```html
   <script>
     window.WIDGET_BASE_URL = document.currentScript.src.replace(/[^/]+$/, '');
   </script>
   <script src="app.js"></script>
   ```

   ```javascript
   // In app.js
   const logo = window.WIDGET_BASE_URL + 'images/logo.png';
   ```

3. **Preload in HTML**: Reference assets in HTML where they get transformed
   ```html
   <link rel="preload" as="image" href="images/logo.png" id="logo-preload">
   <script>
     const logo = document.getElementById('logo-preload').href;
   </script>
   ```

## Choosing an Approach

| Aspect | External | Repository |
|--------|----------|-----------|
| **Control** | You own the hosting | Platform hosts for you |
| **Updates** | Deploy to your server | Commit and push to trigger auto-publish |
| **Cache control** | Full control via `cacheStrategy` | Platform manages |
| **Asset transformation** | Manual (if needed) | Automatic |
| **Suitable for** | Dynamic or frequently updated widgets | Static or infrequently changed widgets |

## Next Steps

* [Widget Definition Reference](widget-schema) — every field of a widget entry
* [Configurable Widgets](configurable-widgets) — Add form fields for the No-Code Builder
* [Repository Layout](project-setup) — How to organize your repository
* [Template repository](https://github.com/gainsight-hub/widgets-repository-template) — Working examples of both hosting approaches: a [simple HTML widget](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets/demo_widget) and a [React widget with Vite build output](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets/react_widget)
