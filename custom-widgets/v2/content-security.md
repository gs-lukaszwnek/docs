---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/content-security.md
description: >-
  Automated pre-publish security scanning the platform runs on every push, with
  no configuration needed; for guidance on which customization approach to
  choose, see Customization Guardrails
---

# Content Security

The platform automatically checks your extension code — widgets, scripts, and stylesheets — for security issues before publishing.

## What the Platform Checks

Every time you push code, the platform runs a series of checks before publishing your extensions. The sections below describe each check and the specific rules enforced.

### Content Scanning

Your code is scanned for patterns that could harm your community.

**Widgets**: For source widgets (directories of files), the platform scans HTML and JavaScript files (`.html`, `.htm`, `.js`, `.mjs`, `.jsx`). CSS, images, and fonts are not scanned since they cannot contain executable patterns.

**Scripts**: JavaScript files are scanned for executable patterns and credential exposure.

**Stylesheets**: CSS files are scanned for unsafe rules or obfuscated content.

The platform checks for:

| Category | What It Detects | Example |
|----------|----------------|---------|
| Crypto mining | Scripts that use visitor devices to mine cryptocurrency | `CoinHive`, WebAssembly mining modules |
| Data exfiltration | Code that collects and sends user data to external servers | Keyloggers, cookie theft, form data harvesting |
| Phishing | Fake login forms or redirects that assign external URLs to `window.location` | Credential harvesting pages, `window.location.href = "https://..."` |
| Obfuscation | Heavily encoded or hidden code that obscures its purpose | `eval(atob(...))`, `eval(String.fromCharCode(...))` chains |
| External script loading | Loading JavaScript from untrusted external sources | `<script src="...">` from unknown domains |
| Credential exposure | Hardcoded secrets, API keys, tokens, or high-entropy strings | AWS keys, private keys, JWTs, `password = "..."` |

:::tip
Loading scripts from popular libraries like `cdn.jsdelivr.net`, `cdnjs.cloudflare.com`, `unpkg.com`, and `esm.sh` is allowed. Only untrusted sources are flagged.
:::

:::tip
Template-style placeholders like `${API_KEY}` are not flagged as credentials. UUIDs and sequential strings are also excluded from credential detection.
:::

### Security Headers

The platform adds browser-level protections to every widget response:

| Header | What It Does |
|--------|-------------|
| Content-Security-Policy | Controls which scripts, styles, and resources your widget can load |
| X-Content-Type-Options | Prevents the browser from guessing file types (blocks MIME sniffing) |
| Referrer-Policy | Limits what URL information is shared when navigating away |
| X-Frame-Options | Controls where your widget can be embedded |

### Size Limits

Each extension has maximum size limits to ensure fast loading and prevent abuse:

* Widget directories: Limited to 100 files and 10 MB total
* Individual files: 2 MB limit
* Widget thumbnail image (`imageSrc`): 512 KB limit
* Scripts and stylesheets: 2 MB per file (same limit as widget content files)

### Path Validation

Widget file paths must stay within the widget directory. Paths containing `../` (directory traversal) or starting with `/` (absolute paths) are rejected.

### Immediate Removal

When you delete an extension, it is immediately removed from the platform and stops being served.

## What This Means for Your Code

Most extension code works without any changes. Here is what to keep in mind:

| What you want to do | Works? | Notes |
|---------------------|--------|-------|
| Inline `<script>` tags (widgets) | Yes | Inline scripts are allowed by the security policy |
| Inline `<style>` tags (widgets) | Yes | Inline styles are allowed by the security policy |
| Relative asset paths (`./styles.css`) | Yes | Standard relative paths work as expected |
| Load from trusted libraries | Yes | `cdn.jsdelivr.net`, `cdnjs.cloudflare.com`, `unpkg.com`, `esm.sh` are allowed |
| Load scripts from unknown domains | No | External scripts from untrusted sources are blocked |
| Use `eval()` | No | Blocked by the Content Security Policy |
| Use `new Function()` from strings | No | Blocked by the Content Security Policy |
| Hardcode API keys or tokens | No | Detected as credential exposure — use Connectors instead |

## What Happens When Content Is Rejected

If the platform detects a security issue in your code, the **entire build fails** — no extensions from that push are published. Previously published content remains available until a clean push succeeds.

The error message includes the category of the issue:

```
Widget 'my-widget' rejected: content policy violation (categories: obfuscation)
```

### How to Resolve Each Category

| Category | What to change |
|----------|---------------|
| `crypto_mining` | Remove cryptocurrency mining scripts entirely |
| `data_exfiltration` | Remove code that sends user data to external servers. If you need to send data, use a [Connector](/connectors/) instead |
| `phishing_redirect` | Remove fake login forms or deceptive redirects. Use your platform's built-in authentication |
| `obfuscation` | Replace obfuscated code with readable source. If you use a bundler, check that its output is not overly minified |
| `external_script_loading` | Load scripts from trusted sources only, or bundle dependencies into your widget directory |
| `credential_exposure` | Remove hardcoded secrets from your code. Use environment variables, a secrets manager, or [Connectors](/connectors/) to handle credentials securely |

## Next Steps

* [Customization Guardrails](customization-guardrails) — Best practices for writing extension code that passes these checks and survives platform updates
* [Error Codes](error-codes) — Full error reference including content scan rejections
* [Build & Publish](build-and-publish) — The publishing pipeline
* [Common Issues](common-issues) — Common issues and solutions
