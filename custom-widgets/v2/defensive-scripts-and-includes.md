---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/defensive-scripts-and-includes.md
description: >-
  Write custom scripts and header/footer includes that survive platform updates
  — load safely, fail gracefully, and avoid collisions with platform code
---

# Writing Defensive Scripts and Includes

Custom scripts and SSI header/footer includes run on every matching page, so they must be written defensively to avoid blocking rendering, breaking the page, or colliding with platform code. Use this guide when you add a global script or a header/footer include.

## Defensive Scripting Practices

* Load scripts asynchronously (`async` or `defer`) to avoid blocking page rendering.
* Wrap script logic in try/catch blocks so failures are silent rather than breaking the page.
* Never assume specific DOM elements exist — check before acting.
* Scope all selectors and variables to avoid collisions with platform code.

## Header and Footer Includes

SSI header and footer includes must stay isolated from platform internals:

* **Keep it isolated.** Header and footer sections should not contain scripts that reach into the main page content. Wrap custom content in dedicated container elements and scope all styling to those containers.
* **Plan for failure.** If the include fails to load, the community should still function normally. Design your header and footer so the page degrades gracefully without them.
* **Review external resources.** If your header or footer loads scripts, fonts, or stylesheets from third-party domains, confirm those sources are trusted and necessary.

## Next Steps

* [Script Definition Reference](scripts) — every field and option for global scripts
* [Stylesheet Definition Reference](stylesheets) — every field and option for global stylesheets
* [Customization Guardrails](customization-guardrails) — why these practices matter and which customization approaches are safe
