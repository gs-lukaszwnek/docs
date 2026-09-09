---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/design-tokens.md
description: >-
  Apply a community's branding — colors, fonts, and other theme values — to your
  widget's CSS using design tokens
---

# Use Design Tokens

To style your widget with a community's branding — colors, fonts, and other theme values — reference design tokens as CSS custom properties in your styles. Use this guide when your widget's appearance should adapt automatically to each community's theme instead of using fixed colors and fonts.

## Use Tokens in CSS

Design tokens are CSS custom properties that reflect the community's branding (colors, fonts, etc.). The platform injects them into your Shadow DOM automatically, so you can use them directly in your widget's styles:

```css
h1 {
  color: var(--color-action-primary-default);
}
```

Use tokens wherever you want your widget to pick up the community's branding — text, backgrounds, borders, icons, accents, shadows.

If you need a shade that has no dedicated token (for example a shadow tint, or a muted/secondary variant), derive it from an existing token instead of hardcoding a color — reuse a token directly, or layer opacity on it with `color-mix()`:

```css
.card {
  box-shadow: 0 2px 8px color-mix(in srgb, var(--color-action-primary-default) 60%, transparent);
}
```

Use only the tokens meant for widgets. The community's own pages have their own tokens — for navigation, sidebars, and feeds — and a widget should not style itself with those. [Design Tokens Reference](design-tokens-reference) marks which is which.

## Next Steps

* [Design Tokens Reference](design-tokens-reference) — The full token catalog and platform default values
* [Widget Runtime Reference](sdk-api-reference) — Properties, methods, and events on the `sdk` object
* [Widget Runtime](core-concepts) — Why widgets receive the `sdk` object and how the runtime model works
