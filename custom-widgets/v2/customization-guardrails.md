---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/customization-guardrails.md
description: >-
  Decision guide for choosing a customization approach before you start — ranks
  no-code configuration, custom widgets, and other options by how safely they
  survive platform updates
---

# Customization Guardrails

There are many ways to customize a Gainsight community — from no-code configuration to custom widgets, scripts, and stylesheets. Not all approaches are equally safe. Some customizations work reliably across platform updates, while others can break when the platform evolves.

This page helps you choose the right approach for each customization need, so your work stays stable and maintainable.

## Customization Tiers

Every customization falls into one of three tiers based on how likely it is to survive platform changes.

### Safe

These approaches are fully supported and designed to be stable across platform updates.

| Approach | Notes |
|----------|-------|
| Out-of-the-box configuration and widgets | No code required — the safest option |
| Custom pages using supported widgets | Built with the No-Code Builder |
| SSI header/footer includes | Isolated from app internals — see [SSI Header and Footer](#ssi-header-and-footer) below |
| Custom widgets, scripts, and stylesheets via Developer Studio | Built with the supported extension framework |

### Use with Caution

These approaches are allowed but need care to avoid fragile results.

| Approach | Conditions |
|----------|------------|
| Custom CSS | Styling only — no structural manipulation. See [Custom CSS](#custom-css) below |
| Custom JavaScript | Simple scripting that does not manipulate the DOM or degrade performance |
| Custom icons | Allowed |

### Avoid

These approaches are not supported and are likely to break during platform updates. If your use case requires one of these, treat it as a product enhancement request rather than a customization.

| Approach | Why It Breaks |
|----------|---------------|
| DOM manipulation (moving, re-parenting, or copying elements) | Conflicts with the platform's rendering engine (Preact/hydration), causing duplicate elements or visual glitches |
| Changing core interaction behavior (like buttons, reply threading, expand/collapse) | These are product behaviors, not styling — changes here create unpredictable side effects |
| JavaScript redirects or non-standard navigation changes | High risk of breaking standard routing |
| Scripts that depend on internal DOM structure or CSS class names | Internal class names and DOM structure change without notice during platform updates |

## Non-Negotiables

These are hard boundaries. Do not attempt these as customizations under any circumstances.

* **Do not move or restructure elements rendered by the platform.** The platform uses Preact with hydration. Third-party scripts that move elements can race against the rendering engine, causing duplicate elements or visual glitches that are difficult to diagnose.
* **Do not implement custom "create post" or "create topic" flows.** These are core platform behaviors with complex state management. If the out-of-the-box flow does not meet your needs, raise it as a product enhancement request.
* **Do not override core UX behaviors via CSS or JavaScript** (for example: non-standard like button behavior, custom reply toggles). These should be evaluated as product changes.

## Custom CSS

**Principle:** CSS is for styling, not for structure or behavior.

### What Works

* Colors, typography, spacing, borders, shadows
* Minor layout tweaks that do not reflow core components
* Component-level styling scoped to a specific component boundary

### What to Avoid

* **Hiding core UI elements** — Risks accessibility issues and unexpected regressions. This includes hiding platform controls like the "send private message" button.
* **Layout rewrites that assume specific DOM structure** — Internal DOM structure changes between platform releases
* **Broad modifications to shared components** — Styles targeting components used across multiple pages or modules can cause unintended side effects

:::warning
Custom CSS that goes beyond styling (hiding elements, restructuring layouts, overriding behavior) is not covered by standard platform support. If the platform updates its UI and your CSS breaks, the fix is on the customization side.
:::

## Third-Party Scripts

Third-party scripts can be loaded on every page, so they must be written defensively and never assume DOM stability.

### What Works

* **Analytics and tag managers** — Non-blocking, async scripts (Google Analytics, Segment, etc.)
* **Isolated UI additions** — Completely self-contained elements like chat widgets or feedback modals that do not touch the platform DOM
* **A/B testing frameworks** — As long as they do not manipulate core platform elements

### What to Avoid

* **Scripts that manipulate the platform DOM** — Moving, reordering, or modifying elements rendered by the platform
* **Scripts that rely on internal CSS class names** — These change without notice during platform updates
* **Heavy library imports** — Loading full frameworks like jQuery or Bootstrap adds significant page weight and can conflict with the platform's own framework

:::info
When the platform ships updates (for example, changes to module loading), third-party scripts that depend on internal structure may break. The platform team cannot revert product improvements to preserve a custom script. If your script breaks after a platform update, the script needs to be updated.
:::

For the practices that keep third-party scripts from blocking, breaking, or colliding with the page, see [Writing Defensive Scripts and Includes](defensive-scripts-and-includes).

## SSI Header and Footer

SSI (Server Side Include) header and footer customization is the recommended approach for brand-level changes like global navigation bars, footers, or announcement banners. It is the safest way to add persistent chrome without interfering with platform internals.

For the practices that keep header and footer includes isolated and resilient, see [Writing Defensive Scripts and Includes](defensive-scripts-and-includes).

## Writing Durable Extension Code

Choosing a safe customization tier is only half the picture. A few habits keep the code you do write clean enough to publish and stable enough to last:

* **Readable code publishes reliably.** The automatic security scan flags heavily obfuscated code, so keep your source readable. Standard minification from bundlers like webpack, Vite, or esbuild is fine.
* **Bundled dependencies outlast external URLs.** Including a library in your repository avoids both the external-script check and the fragility of a third-party URL changing underneath you.
* **Connectors keep credentials out of the browser.** Routing external API calls through [Connectors](/connectors/) means no keys or tokens ever live in extension code — which is also what the credential scan enforces.
* **Relative asset paths stay portable.** Referencing CSS, JavaScript, and images with relative paths keeps them valid wherever the platform serves your extension.

## What Happens When Customizations Break

The platform evolves continuously. When a platform update affects a customization:

* **Safe-tier customizations** continue to work. The platform maintains backward compatibility for supported configuration and the extension framework.
* **Caution-tier customizations** (scoped CSS, simple scripts) generally survive, but may occasionally need minor adjustments.
* **Avoid-tier customizations** (DOM manipulation, internal class dependencies) are likely to break and require rework. The platform team will not revert product changes to preserve unsupported customizations.

When in doubt about whether a customization approach is safe, use the supported [Extensions](/custom-widgets/v2/) framework — widgets, scripts, and stylesheets are designed to survive platform updates.

## Next Steps

* [Writing Defensive Scripts and Includes](defensive-scripts-and-includes) — Practices for scripts and header/footer includes that survive platform updates
* [Content Security](content-security) — Automatic security checks applied to your code
* [Script Definition Reference](scripts) — Add custom JavaScript to community pages
* [Stylesheet Definition Reference](stylesheets) — Add custom CSS to community pages
* [Connectors](/connectors/) — Route external API calls securely through the backend
